---
layout: post
title: RxSwift + MVVM 구구단작성하기
tags: [RxSwift]
sitemap :
  changefreq : daily
  priority : 1.0
---

- MVVM 패턴을 익히기 위해서 간단한 구구단을 작성해봄.

```c
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {

    @IBOutlet weak var numberTextField: UITextField!
    @IBOutlet weak var resultLabel: UILabel!
    
    let viewModel = ViewModel()
    let disposeBag = DisposeBag()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.bind()
    }
    
    func bind() {
        numberTextField.rx.text.orEmpty
            .bind(to: viewModel.numberObserver)
            .disposed(by: disposeBag)
        
        viewModel.multiplicationObserver
            .bind(to: resultLabel.rx.text)
            .disposed(by: disposeBag)
    }
}
```

```c
import Foundation
import RxSwift

protocol ViewModelType {
    var numberObserver: AnyObserver<String> { get }
    var multiplicationObserver: Observable<String> { get }
}

class ViewModel: ViewModelType {
    
    let numberObserver: AnyObserver<String>
    let multiplicationObserver: Observable<String>
    
    let disposeBag = DisposeBag()
    
    init() {
        let number = PublishSubject<String>()
        let multiplication = BehaviorSubject<String>(value: "")
        
        numberObserver = number.asObserver()
        multiplicationObserver = multiplication.asObserver()
        
        number.subscribe(onNext: { str in
            if Int(str) != nil {
                var result = ""
                for i in 1..<10 {
                    result += "\(String(Int(str)! * i))\n"
                }
                multiplication.onNext(result)
            } else {
                multiplication.onNext("")
            }
        }).disposed(by: disposeBag)
    }
}
```
