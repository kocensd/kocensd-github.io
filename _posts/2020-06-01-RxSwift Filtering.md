---
layout: post
title: RxSwift Filtering
tags: [RxSwift]
sitemap :
  changefreq : daily
  priority : 1.0
---

- Rx의 Filtering operator

```c
//Filtering Observables
var disposeBag = DisposeBag()

let test = Observable.of(1,2,3,4,5)

print("------filter------")
// filter - 어떤 조건을 만족시키지 않는 항목들을 필터링해서 재배출 해야한다면
test
    .filter { $0 % 2 == 0 }
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 2, 4

print("------first------")
// first - 첫번째 항복만 재배출 해야 한다면 (First는 방출 시점에서 특정항목을 반환하는 차단함수로 구현될 수도 있기때문에 take(1)와 ElememtAt(0)를 사용하는것이 더 좋다)
test
    .first()
    .subscribe({  // onNext: 를 했을때 에러가 나는 이유??
        print($0)
    }).disposed(by: disposeBag)

print("------take------")
// take - 처음 몇개의 항목들만 재배출 해야한다면
test
    .take(2)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 1, 2

print("------takeLast------")
// takeLast - 마지막 항목만 재배출 해야한다면
test
    .takeLast(1)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 5

print("------elementAt------")
// elementAt - 몇번째 위치한 항목만 재배출 해야한다면
test
    .elementAt(3) // 0부터 시작
    .subscribe(onNext: {
    print($0)
    }).disposed(by: disposeBag) // 4

print("------skip------")
// 재배출할 항목들이 처음 몇개 이후의 것들일 경우
// skip - 처음 몇개의 항목들이 이후의 것
test
    .skip(2) //2번째까지 skip하고 나머지들은 방출
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 3, 4, 5

print("------skip(time)------")
test
    .skip(1.0, scheduler: MainScheduler.instance)
    .subscribe(onNext: {
    print($0)
})

print("------skipWhile------")
// skipWhile - 특정조건을 만족시킨 이후의 것들이라면
test
    .skipWhile { $0 % 2 != 0 } //처음으로 이 조건을 통과하지 못한 이벤트가 나타나면 그 이후의 이벤트는 모두 방출된다.
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 2, 3, 4, 5   ex) 1,1,1,4,5 -> 4, 5

print("------skipUntil------")
let A = PublishSubject<Int>()
let B = PublishSubject<Int>()

// skipUntil - 두번째 Observable이 항목을 배출한 이후에 첫번째 Observable를 배출한다면
A
    .skipUntil(B)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // ""

A.onNext(1) // ""
B.onNext(2) // B가 이벤트가 전달됬으므로 이 다음 A부터는 이벤트가 방출된다.
A.onNext(3) // 3
A.onNext(4) // 3, 4

print("------Debounce VS Throttle------")
// debounce - 특정 시간이 지나고서 배출된 항목들만 재배출할때
// Throttle - 이벤트가 발생되고나서 시간동안 다른이벤트를 무시함
test
    .debounce(2.0, scheduler: MainScheduler.instance)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 2초 지난 뒤에 호출됨
    
test
    .throttle(2.0, scheduler: MainScheduler.instance)
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // 호출되고나서 2초 뒤에 다음이벤트를 받음

print("------distinctUntilChanged------")
// distinctUntilChanged - 중복된 항목이 연이여서 배출될때
test
    .distinctUntilChanged()
    .subscribe(onNext: {
        print($0)
    }).disposed(by: disposeBag) // ex) 1,2,2,4,5 -> 1,2,4,5 | 1,2,1,4,5 -> 1,2,1,4,5 -> 연이여서 나오지 않았기 때문에 그대로 다 출력됨


```
