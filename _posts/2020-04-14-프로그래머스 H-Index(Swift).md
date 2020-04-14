###### 문제 해설

- 일단 H-Index가 무엇인지부터 알면 간단하게 풀 수 있는 문제다.
* 전체 논문중에서 피인용수가 논문수보다 작아지기 시작하는 숫자가 H-Index가 되는것이다.
ex) [3, 0, 6, 1, 5]
index 논문 인용횟수를 정렬해서 나타냄
0      6
1      5
2      3
3      1   ---> 이때가 피인용수가 논문수보다 작아지는 구간이다. 그래서 H-Index의 값은 3이 되는것이다
4      0

```c
func solution(_ citations:[Int]) -> Int {
   let arr = citations.sorted(by: >)
    
    for (i, ar) in arr.enumerated() {
        if i >= ar {
            return i
        }
    }
    
    return citations.count //논문의 수와 인용횟수가 같다는 조건이 있을 수 있으니 그때는 논문 갯수를 return 시켜준다.
}
```
