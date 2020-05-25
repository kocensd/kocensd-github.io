---
layout: post
title: ReactorKit으로 Memo앱 만들기(1)
sitemap :
  changefreq : daily
  priority : 1.0
---

- Realm을 사용하여 curd가 가능한 간단한 샘플 메모앱을 만들어보기 (화면은 이런식으로 기본구성을 하였습니다)
- <img src="https://user-images.githubusercontent.com/45751308/82796300-46f33c80-9eb0-11ea-83ea-e7a29ab85783.png" width="40%"></img>

- ReadViewController.swift파일과 ReadViewControllerReactor 파일을 만들어줍니다.

```c
//ReadViewController 

func bind(reactor: ReadViewControllerReactor) {

  //action
  Observable.just(Void())
    .map { Reactor.Action.getMemos }
    .bind(to: reactor.action)
    .disposed(by: self.disposeBag)
    
  //state
  reactor.state.map { $0.memos }
    .bind(to: tableView.rx.items(cellIdentifier: "cell")) { indexPath, memos, cell in
      cell.textLabel?.text = memos.title
      cell.detailTextLabel?.text = memos.date
  }.disposed(by: disposeBag)

}
```

```c
//ReadViewControllerReactor 
  enum Action {
    case getMemos
  }
  enum Mutation {
    case getMemos([Memo])
  }
  struct State {
    var memos: [Memo] = []
  }
  
  let initialState = State()
  
  func mutate(action: ReadViewControllerReactor.Action) -> Observable<ReadViewControllerReactor.Mutation> {
    switch action {    
      case .getMemos:
        let memos = DB.shared.load() 
        return Observable.just(Mutation.getMemos(memos))
    }
  }
  
   func reduce(state: ReadViewControllerReactor.State, mutation: ReadViewControllerReactor.Mutation) -> ReadViewControllerReactor.State {
     switch mutation {
       var newState = state
       case let .getMemos(memo):
         newState.memos = memo
         return newState
     }
   }
```











