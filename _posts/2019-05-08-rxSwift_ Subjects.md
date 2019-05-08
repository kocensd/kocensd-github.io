- Subjects: Observable 과 Observer 두 역할을 수행한다.

종류는 총 4가지
- PublishSubject
- ReplaySubject
- BehaviorSubject
- Variable

1. PublishSubject
- 구독이후에 Observable의 이벤트를 받는다.

```c
let subject = PublishSubject<String>()
// 2
subject.onNext("Is anyone listening?")

// 3
let subscriptionOne = subject
    .subscribe(onNext: { (string) in
        print(string)
})

// 4
subject.on(.next("1"))        //Print: 1

// 5
subject.onNext("2")        //Print: 2

```
2. ReplaySubject
- bufferSize크기만큼 구독 전에 발생한 이벤트를 버퍼에 넣고 이벤트를 받는다.

3. BehaviorSubject
- 

4. Variable
