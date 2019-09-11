---
layout: post
title: '[기술 면접 질문] 기술 면접 예상 질문 대비하기 - 스프링(Spring)편'
subtitle: '기술 면접 예상 질문 대비하기 - 스프링(Spring)편'
date: 2017-10-01
author: heejeong Kwon
cover: '/images/basic-concepts-of-development/basic-concepts-of-development-main.png'
tags: 면접 기본개념 스프링 spring
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---
기존의 collectionview에 rx형태로 변경을 진행

```c
class MainViewController: UIViewController, WKUIDelegate,WKNavigationDelegate {
    
    var mainView: UIView!
    var mainWebview: WKWebView!
    var maincollectionView: UICollectionView!
    var bottomMenus = [BotMenu]()
    var bottomSpaceView: UIView!
    var cellWidth: CGFloat = 0.0
    
    var disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        viewInit()
        viewload()
        collectionViewInit()
    }
    
    func viewInit() {
        bottomSpaceView = UIView()
        bottomSpaceView.backgroundColor = .white
        self.view.addSubview(bottomSpaceView)
        bottomSpaceView.snp.makeConstraints { make in
            if #available(iOS 11.0, *) {
                make.top.equalTo(self.view.safeAreaLayoutGuide.snp.bottom)
            } else {
                make.top.equalTo(self.bottomLayoutGuide.snp.bottom)
            }
            make.leading.bottom.trailing.equalTo(0)
        }
        
        let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
        layout.sectionInset = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
        layout.minimumLineSpacing = 0
        layout.minimumInteritemSpacing = 0
        cellWidth = screenWidth / CGFloat(bottomMenus.count)
        layout.itemSize = CGSize(width: cellWidth, height: 40)
        maincollectionView = UICollectionView(frame: CGRect.zero, collectionViewLayout: layout)
        maincollectionView.backgroundColor = .white
        maincollectionView.register(BottomMenuCell.self, forCellWithReuseIdentifier: BottomMenuCell.ID)
        self.view.addSubview(maincollectionView)
        maincollectionView.snp.makeConstraints { make in
            make.leading.trailing.equalTo(0)
            make.bottom.equalTo(bottomSpaceView.snp.top)
            make.height.equalTo(40)
        }
        
        mainView = UIView()
        self.view.addSubview(mainView)
        mainView.snp.makeConstraints { make in
            make.leading.trailing.equalTo(0)
            if #available(iOS 11.0, *) {
                make.top.equalTo(self.view.safeAreaLayoutGuide.snp.top)
            } else {
                make.top.equalTo(self.topLayoutGuide.snp.bottom)
            }
            make.bottom.equalTo(maincollectionView.snp.top)
        }
        
        mainWebview = WKWebView()
        mainWebview.uiDelegate = self
        mainWebview.navigationDelegate = self
        self.mainView.addSubview(self.mainWebview)
        mainWebview.snp.makeConstraints { make in
            make.top.leading.bottom.trailing.equalTo(self.mainView)
        }
    }
    
    func collectionViewInit() { //rx형태로 bottomMenus에 들어있는 하단 메뉴를 가져와서 보여준다.
        let menus = Observable.just(
            bottomMenus.map { $0 }
        )

        menus.bind(to: maincollectionView.rx.items(cellIdentifier: BottomMenuCell.ID, cellType: BottomMenuCell.self)) {
            (row, element, cell) in
            cell.bottomImageView?.sd_setImage(with: URL(string: element.path), completed: nil)
        }.disposed(by: disposeBag)
        
        maincollectionView.rx
            .modelSelected(BotMenu.self)
            .subscribe({ (item) in
                print(item.element?.path ?? "")
                let pushVC = PushViewController()
                self.present(pushVC, animated: true, completion: nil)
            }).disposed(by: disposeBag)
    }
    
    func viewload() {
        let url = URL(string: "url주소")
        let request = URLRequest(url: url!)
        mainWebview.load(request)
    }
}
```

```c
class BottomMenuCell: UICollectionViewCell {
    static  let ID = "cell"
    
    var bottomImageView: UIImageView?
    
    override func layoutSubviews() {
        super.layoutSubviews()
    }
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        self.bottomImageView = UIImageView()
        self.bottomImageView?.contentMode = .scaleAspectFit
        self.addSubview(self.bottomImageView!)
        self.bottomImageView?.snp.makeConstraints { make in
            make.leading.top.trailing.bottom.equalTo(0)
        }
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}

```
