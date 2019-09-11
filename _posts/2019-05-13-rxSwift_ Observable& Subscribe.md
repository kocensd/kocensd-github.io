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

- Observable

```c
let disposeBag = DisposeBag()

//just 하나의 요소를 포함하는 Observable Sequence생성
let observable = Observable.just(1)

//of 주어진 값들에 대해서 생성
let observable1 = Observable.of(1, 2, 3) //Observable<Int>
let observable2 = Observable.of([1, 2, 3]) // Observable<[Int]>

//from array요소들로 생성
let observable3 = Observable.from([1, 2, 3]) // Observable<Int>
let observable4 = Observable.from([1:"Hello",2:"World"]) // Observable<key:value>


observable4.subscribe(onNext: { (str) in
    print(str.value)
}, onError: { (error) in
    print(error)
}, onCompleted: {
    print("completed")
}).disposed(by: disposeBag)
```

- observable 연산

```c
//1. map 변형
_ = Observable.of(1, 2, 3, 4)
    .map { value in
    return value * 10
    }.subscribe(onNext: { (result) in
        print(result) // 1, 2, 3, 4
    }).disposed(by: disposeBag)

//2. flatMap 결합
let flatmap1 = Observable.of(1,3)
let flatmap2 = Observable.of(2,4)

let plusFlatmap = Observable.of(flatmap1, flatmap2)

plusFlatmap
    .flatMap{ value in
    return value
    }.subscribe(onNext: { result in
        print(result) // 1, 3, 2, 4
    }).disposed(by: disposeBag)

//3. scan 초기값이 필요하고 값 합산
_ = Observable.of(1, 2, 3, 4, 5)
    .scan(0, accumulator: {
        return $0 + $1
    }).subscribe(onNext: { (result) in
        print(result) // 1, 3, 6, 10, 15
    }).disposed(by: disposeBag)

//4.1 filter 조건에 따른 데이터 추출
_ = Observable.of(1, 2, 3, 4, 5, 6)
    .filter {
        return $0 % 2 == 0
    }.subscribe(onNext: { (result) in
        print(result)
    }).disposed(by: disposeBag)

//4.2 DistinctUntilChanged 변화가 있을때만 발생
_ = Observable.of(1, 2, 2, 2, 3, 3, 4, 5)
    .distinctUntilChanged()
    .subscribe(onNext: { (result) in
        print(result) // 1, 2, 3, 4, 5
    }).disposed(by: disposeBag)

```

//5. combineLatest 결합

```c
var a = BehaviorRelay(value: 1)
var b = BehaviorRelay(value: 2)

let c = Observable
    .combineLatest(a.asObservable(), b.asObservable()) {
        $0 + $1
    }
    .filter { $0 >= 0 }
    .map { $0 }

c.subscribe(onNext: {
    print($0)
})
a.accept(3) // 5
a.accept(8) // 10
a.accept(-8) // filter에 걸러짐

```
