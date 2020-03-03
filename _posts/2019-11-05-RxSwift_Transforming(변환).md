---
layout: post
title: RxSwift Transforming
sitemap :
  changefreq : daily
  priority : 1.0
---

- RxSwift 변환

```c

// - buffer
// - timeSpan: buffer 에 저장되는 시간간격, - count: 버퍼에 저장되는 최대 이벤트 갯수
let bufferObservable = Observable<Int>.interval(1.0, scheduler: MainScheduler.instance)
bufferObservable.buffer(timeSpan: 3, count: 3, scheduler: MainScheduler.instance)
    .subscribe(onNext: {
    print($0)
    }).disposed(by: disposeBag)


// - flatMap
let timer = Observable<Int>
        .interval(1.0, scheduler: MainScheduler.instance)
        .take(5)

timer.flatMap { (num) -> Observable<String> in
    return Observable<String>.create { observer in
        observer.on(.next("A\(num)"))
//        observer.on(.next("B"))
        return Disposables.create()
    }
}.subscribe(onNext: {
    print($0)
}).disposed(by: disposeBag)


struct Student {
    var score: BehaviorSubject<Int>
}

let 학생1 = Student(score: BehaviorSubject(value: 80))
let 학생2 = Student(score: BehaviorSubject(value: 90))
let test = Student(score: BehaviorSubject(value: 10))

let student = PublishSubject<Student>()

// - flatMapLatest
student.flatMapLatest { $0.score }
    .subscribe(onNext:{
    print($0)
    }).disposed(by: disposeBag)

student.onNext(학생1)
학생1.score.onNext(85)

student.onNext(학생2)
학생1.score.onNext(95) //출력 안됨 학생2가 마지막으로 새로 들어왔기 때문에

학생2.score.onNext(100)
학생2.score.onNext(20)

student.onNext(test)
test.score.onNext(50)

학생2.score.onNext(200) //출력안됨 test가 마지막으로 새로 들어왔기 때문에


//// - groupBy
let groupByObservable = Observable.range(start: 1, count: 10)

groupByObservable.groupBy { (value) -> Bool in
    return value % 2 == 0
}.flatMap({ (grouped) -> Observable<[Int]> in  //이걸 왜 써줘야할까 ??
    return grouped.toArray()
}).subscribe(onNext: { group in
    print(group)
}).disposed(by: disposeBag)



//// - map: 변환시킨다.
let numbserObservable = Observable<Int>.just(10) //Int -> String으로 변환
let observableMap = numbserObservable.map { String($0) }
let observableFlatMap = numbserObservable.flatMap { Observable<String>.just(String($0)) }

observableMap.subscribe(onNext:{ element in
    print("map: \(element)")  // String형
})
observableFlatMap.subscribe(onNext: { element in
    print("flatMap: \(element)")  // String형
})



// - scan: 하나씩 더할때 증가된 합 scan(초기값, 연산자)
Observable.of(1,2,3,4,5).scan(1, accumulator: *)
.subscribe(onNext: {
    print($0)
}).disposed(by: disposeBag)

Observable.of(10,20,30,40).scan(100, accumulator: -)
.subscribe(onNext: {
    print($0)
}).disposed(by: disposeBag)


```
