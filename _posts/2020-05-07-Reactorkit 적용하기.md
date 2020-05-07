---
layout: post
title: Reactorkit
sitemap :
  changefreq : daily
  priority : 1.0
---

- 위키백과 api를 호출하여 테이블 뷰에 목록 보여주기
- 추가 개발사항 (통신클래스)

```c
class ViewController: UIViewController, StoryboardView {
    @IBOutlet weak var tableView: UITableView!
    @IBOutlet weak var searchBar: UISearchBar!
    @IBOutlet weak var testButton: UIButton!
    
    var disposeBag = DisposeBag()
    var testUrl = "https://en.wikipedia.org/w/api.php?action=opensearch&limit=10&namespace=0&format=json&search=Apple"

    override func viewDidLoad() {
        super.viewDidLoad()
        self.reactor = ViewControllerReactor()
    }
    
    func bind(reactor: ViewControllerReactor) {
        searchBar.rx.text
            //문제점1. searchBar에 검색 할때마다 호출하면 안되기 때문에 맨 마지막 타이핑이 끝난 이후에 통신처리를 해줘야한다.
            // throttle vs debounce
//            .throttle(.seconds(5), scheduler: MainScheduler.instance)
            .debounce(.seconds(2), scheduler: MainScheduler.instance)
            .map { Reactor.Action.getData($0) }
            .bind(to: reactor.action)
            .disposed(by: disposeBag)
        
        // State
        reactor.state.map { $0.repos }
          .bind(to: tableView.rx.items(cellIdentifier: "cell")) { indexPath, repo, cell in
            cell.textLabel?.text = repo
          }
          .disposed(by: disposeBag)
        
        // View
        tableView.rx.itemSelected
            .subscribe(onNext: { [weak self, weak reactor] indexPath in
                guard let `self` = self else { return }
                self.view.endEditing(true)
                self.tableView.deselectRow(at: indexPath, animated: false)
                guard let page = reactor?.currentState.urls[indexPath.row] else { return }
                if let url = URL(string: "https://en.wikipedia.org/wiki/\(page)") {
                    UIApplication.shared.open(url)
                }
            })
            .disposed(by: disposeBag)
    }
    
}
```


```c
class ViewControllerReactor: Reactor {

    enum Action {
        case getData(String?)
    }
    
    enum Mutation {
        case setQuery(String?)
        case setRepos([String], [String])
    }
    
    struct State {
        var query: String?
        var repos: [String] = []
        var urls: [String] = []
    }
   
    let initialState = State()
    
    func mutate(action: ViewControllerReactor.Action) -> Observable<ViewControllerReactor.Mutation> {
        switch action {
        case let .getData(query):
            //concat: 두 개 이상의 Observable 순서대로 연이어 배출한다.
            return Observable.concat([
                
              // 1) set current state's query (.setQuery)
              Observable.just(Mutation.setQuery(query)),

              // 2) call API and set repos (.setRepos)
              self.search(query: query, page: 1)
                .map { Mutation.setRepos($0, $1) },
            ])
        }
        
    }
    
    func reduce(state: ViewControllerReactor.State, mutation: ViewControllerReactor.Mutation) -> ViewControllerReactor.State {
        var newState = state
        switch mutation {
        case let .setQuery(query):
            newState.query = query
        case let .setRepos(repos, nextPage):
            newState.repos = repos
            newState.urls = nextPage
        }
        return newState
    }
    
    private func url(for query: String?, page: Int) -> URL? {
        guard let query = query, !query.isEmpty else { return nil }
        return URL(string: "https://en.wikipedia.org/w/api.php?action=opensearch&limit=10&namespace=0&format=json&search=\(query)")
//        return URL(string: "https://api.github.com/search/repositories?q=\(query)&page=\(page)")
    }
    
    private func search(query: String?, page: Int) -> Observable<(repos: [String], nextPage: [String])> {
        let emptyResult: ([String], [String]) = ([], [])
        guard let url = self.url(for: query, page: page) else { return .just(emptyResult) }
        
//        return URLSession.shared.rx.json(url: url)
//            .map { json -> ([String], [String]) in
//                let data = json as! NSArray
//                let textArr = data[1] as! [String]
//                let urlArr = data[3] as! [String]
//                return (textArr, urlArr)
//        }
            
        return RxAlamofire.requestJSON(.get, url)
            .map { json -> ([String], [String]) in
                let data = json.1 as! NSArray
                let textArr = data[1] as! [String]
                let urlArr = data[3] as! [String]
                return (textArr, urlArr)
                
        }
        .do(onError: { error in
            if case let .some(.httpRequestFailed(response, _)) = error as? RxCocoaURLError, response.statusCode == 403 {
                print("⚠️ GitHub API rate limit exceeded. Wait for 60 seconds and try again.")
            }
        })
            .catchErrorJustReturn(emptyResult)
    }
    
}
```
