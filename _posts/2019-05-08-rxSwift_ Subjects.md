- Subjects: Observable 과 Observer 두 역할을 수행한다.

종류는 총 4가지
- PublishSubject
- ReplaySubject
- BehaviorSubject
- Variable

1. PublishSubject
- 구독이후에 Observable의 이벤트를 받는다. 예제에서 보면 .subscribe(구독)전에 test1을 값을 추가했지만 출력되지 않고 구독이후에
on한 값들만 출력이 된다.


```c
let subject = PublishSubject<String>()

subject.onNext("test1") //출력되지 않음

_ = subject
  .subscribe(onNext: { (string) in
    print(string)
})

subject.onNext("test2") // test2 출력
subject.onNext("test3") // test3 출력

```
2. ReplaySubject
- bufferSize크기만큼 구독 전에 발생한 이벤트를 버퍼에 넣고 이벤트를 받는다.

3. BehaviorSubject
- 

4. Variable
