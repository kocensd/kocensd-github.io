---
layout: post
title: RxSwift Create
sitemap :
  changefreq : daily
  priority : 1.0
---

- http://reactivex.io에 있는 Creating Observables를 살펴보자

```c
import UIKit
import RxSwift
import RxCocoa

let disposeBag = DisposeBag()

// - create
let createObservable = Observable<String>.create { element -> Disposable in
    element.on(.next("A"))
    element.on(.next("B"))
    element.on(.completed)
    return Disposables.create()
}
createObservable.subscribe(onNext: { element in
    print("create: \(element)")
}, onCompleted: {
    print("completion")
})


// - defer: 쉽게 생각해서 lazy같다고 보면된다. subscribe가 발생하기 전까지 생성하지 않는다.
let deferObservable = Observable<Int>.deferred {
    return Observable.of(1,2,3)
}
deferObservable.subscribe(onNext: {
    print("defer: \($0)")
})


// - empty, naver, throw : 언제 쓸려나??
let emptyObservable = Observable<String>.empty()
emptyObservable.subscribe(onNext: {
    print("empty: \($0)")
    }).disposed(by: disposeBag)


// - from: Array나 Dictionary들을 만들때 사용한다.
let arrayAny: [Any] = ["A", 1, "C", 2, "E",]
Observable.from(arrayAny)
    .subscribe(onNext: { element in
        print("from: \(element)")
    }).disposed(by: disposeBag)

let dic = ["A":1, "B":2, "C":3] //순서상관 없이 출력된다.
Observable.from(dic)
    .subscribe(onNext: { element in
        print("from: \(element)")
        print(element.key)
        print(element.value)
    }).disposed(by: disposeBag)


// - of: 같은 타입을 만들때 사용한다.
let arrayString = ["A","B","C","D","E"]
Observable.of(arrayString)
    .subscribe(onNext: {    //생략해주고 $0 으로 사용가능하다.
        print("of: \($0)")
    }).disposed(by: disposeBag)


// - interval: 특정 시간 간격으로 Observable생성
//let timerObservable = Observable<Int>.interval(1.0, scheduler: MainScheduler.instance)
//timerObservable.subscribe(onNext: {
//    print($0)
//    }).disposed(by: disposeBag)

// - just: 단 한개의 파라미터를 받아서 Observable생성
let one = 1
let two = 2
let three = 3

let justObservable = Observable.just(2)
justObservable.subscribe(onNext: {
    print("just: \($0)")
    }).disposed(by: disposeBag)


// - range: start와 count를 가지고 원하는 갯수만큼 Observable를 생성할 수 있다.
let rangeObservable = Observable.range(start: 0, count: 10)
rangeObservable.subscribe(onNext: {
    print("range: \($0)")
    }).disposed(by: disposeBag)


// - repeat: 계속 반복 take(_ count: Int)로 조절 가능하다.
let repeatString = "반복"
Observable.repeatElement(repeatString)
    .take(6)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag)


// - Timer: 지정된 delay 후 Observable 한번 생성 (interval이랑은 다른거임)
Observable<Int>.timer(3.0, scheduler: MainScheduler.instance).subscribe(onNext: {
    print($0)
    }).disposed(by: disposeBag)
```
