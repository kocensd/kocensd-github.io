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
