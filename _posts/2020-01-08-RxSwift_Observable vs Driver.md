---
layout: post
title: Observable vs Driver
sitemap :
  changefreq : daily
  priority : 1.0
---

- RxSwift 예제를 보면서 공부하다보니 Observable<String> 형태가아닌 Driver<String>가 나오는데 이걸 왜 사용할까??

- Observable사용 (처음에는 그냥 이렇게 사용했다)

```c
let disposeBag = DisposeBag()
let observable1 = Observable.of("a", "b", "c") //Observable<String>

observable1.subscribe(onNext: { (str) in
    print(str)
}, onError: { (error) in
    print(error)
}, onCompleted: {
    print("completed")
}).disposed(by: disposeBag)
```

- 하지만 Observable은 상황에 따라서 background나 Main쪽에서 발생되게 할 수 있기 때문에 발생될 지점을 정해주는것이 좋다.
기본적으로 background에서 돌고 화면 UI에 관여를 한다면 .observeOn(MainScheduler.instance)으로 Main에서 동작을 시켜주는 것이다.
하지만 매번 이렇게 선언해주면 불편하기 때문에 Driver를 사용하는 것이다. Driver은 MainThread에서만 동작한다.

```c
@IBOutlet weak var idField: UITextField!

let textValue: Driver<String> = Driver.of("1")

textValue
    .drive(idField.rx.text)
    .disposed(by: disposeBag)
```
textField에 값을 변경한다거나, view를 이동시키는 값등 화면 UI에 관련된것이라면 Driver를 사용하는게 좀더 효율적이다.
