---
layout: post
title: Swift 알고리즘(Array편)
sitemap :
  changefreq : daily
  priority : 1.0
---

- 알고리즘에서 풀다보니 배열이 가장 많이 나오는거 같아서 한번 내용 정리를 해봄

```c
//배열

//생성
let array1: [Int] = [1, 2, 3, 4, 5]
let array2: [Int] = []
let array3 = Array(repeating:0, count: 10)

//검사
array1.isEmpty //false
array1.count // 5

//액세스
array1[0] // 1
array1.firstIndex //Optional(1)
array1.last //Optional(5)
array1[2..<array1.endIndex] //[3, 4, 5]
array1.randomElement() //Optional(randomValue)

//추가 + 결합 - append(contentsOf: [])
var test = ["a", "b", "c"]
test.append("d") //단일추가
test.append(contentsOf: ["e","f"]) //배열로 추가 ["a", "b", "c", "d", "e", "f"]
test.insert("1", at: 2) //중간에 추가 at값은 index ["a", "b", "1", "c", "d", "e", "f"]
test.replaceSubrange(1...2, with: repeatElement("3", count: 2)) //["a", "3", "3", "c", "d", "e", "f"]

//제거
var test1 = [1, 2, 3, 4, 5]
test1.remove(at: 0) //[2, 3, 4, 5]
test1.removeFirst() //[3, 4, 5]
test1.removeLast() //[3, 4]
test1.removeLast(2) //갯수다 1하면 4가 삭제되고 2하면 3,4가 모두 삭제된다.

var test2 = [1, 2, 3, 4, 5]
test2.removeSubrange(1...3) //[1, 5]

var test3 = "hello swift"
var model: Set<Character> = ["o", "i"]
test3.removeAll(where: { model.contains($0) }) //hell swft


//탐색
var test4 = [1, 2, 3, 4, 5]
test4.contains(1) //true
test4.contains { $0 == 6 } //false
test4.allSatisfy { $0 > 1 } //모든 요소가 같은지 체크

test4.first(where: { $0 > 1 }) //조건에 만족하는 첫번째 요소를 반환 //Optional(2)
test4.firstIndex(of: 3) //3이랑 만족하는 첫번째 index값 //Optional(2)
test4.firstIndex(where: { $0 > 3 }) //index값 //Optional(3)

test4.last(where: { $0 < 4 }) //조건에 만족하는 마지막 요소 반환 //Optional(3)
test4.lastIndex(of: 3) //3이랑 만족하는 마지막 index값
test4.lastIndex(where: { $0 > 2 }) //조건에 만족하는 마지막 index값 //Optional(4)

test4.min() //최소값
test4.max() //최대값

var test5 = ["test1": 1, "test2": 2, "test3": 3]
test5.max { a, b in a.value < b.value } //Optional((key: "test3", value: 3))
test5.max { a, b in a.value > b.value } //Optional((key: "test1", value: 1))
test5.min { a, b in a.value < b.value } //Optional((key: "test1", value: 1))
test5.min { a, b in a.value > b.value } //Optional((key: "test3", value: 3))


//요소 선택
var test6 = [1, 2, 3, 4, 5, 6]
- !! test6.prefix(2) //지정된 길이(index아님)까지 요소를 반환함 [1, 2]
test6.prefix(through: 2) //지정된 위치까지 [1, 2, 3]
test6.prefix(upTo: 2) //지정된 위치를 포함X [1, 2]

- !! test6.suffix(2) //[5, 6]
test6.suffix(from: 2) //지정된 위치에서부터 끝까지 [3, 4, 5, 6]


//요소 제외
var test7 = [1, 2, 3, 4, 5, 6]
test7.dropFirst(3) //앞에서부터 3만큼 제외하고 반환
test7.dropLast(3) //뒤에서부터 3만큼 제외하고 반환
test7.drop(while: { $0 % 2 != 0 }) // ???


//배열 변형
let test8 = ["AAA", "BB", "CCCC", "D"]
test8.map { $0.lowercased() } //["a", "b", "c", "d"]
test8.map { $0.count } //[3, 2, 4, 1]

let test9 = [1, 2, 3, 4, 5]
test9.map { Array(repeating: $0, count: $0) } //[[1], [2, 2], [3, 3, 3], [4, 4, 4, 4], [5, 5, 5, 5, 5]]
//test9.flatMap { Array(repeating: $0, count: $0) } //새로운 배열로 출력 해줌 [1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 5, 5]
-> compactMap으로 변경됨

let test10 = ["1", "두번째", "3", "네번째", "5"]
test10.map { Int($0) } //[Optional(1), nil, Optional(3), nil, Optional(5)]
test10.compactMap { Int($0) } //[1, 3, 5]

let test11 = [1, 2, 3, 4, 5]
test11.reduce(0, { $0 + $1 }) //시퀀스 요소를 결합 (맨앞에 초기값, 모든요소 +)

let test12 = "abcdbaacbdd"
test12.reduce(into: [:]) { counts, letter in //시퀀스 요소를 결합하여 새로운형태로 반환 ["d": 3, "b": 3, "a": 3, "c": 2]
    counts[letter, default: 0] += 1
}


//배열 정렬
var test13 = [1, 3, 5, 2, 8, 6]
test13.sort() //[1, 2, 3, 5, 6, 8]
test13.sort(by: <) //[1, 2, 3, 5, 6, 8]
test13.sort(by: >) //[8, 6, 5, 3, 2, 1]

let test13Sorted = test13.sorted() //sort()는 원본 배열을 정렬시켜버리지만 sorted()는 정렬된 배열로 리턴시켜주고 원본 배열은 그대로 있다.
let test13SortedBy = test13.sorted(by: >) //이것도 마찬가지 대신 by로 내림차순으로 정렬시킬 수 있다. 원본 배열은 그대로

test13.reverse() //[6, 8, 2, 5, 3, 1]
let test13Reversed = test13.reversed() //ReversedCollection<Array<Int>>(_base: [6, 8, 2, 5, 3, 1])


//요소 분리 및 결합
let str1 = "test1 test2  test3"
str1.split(separator: " ") //단순하게 " "기준으로 잘라서 배열을 만들어줌 ["test1", "test2", "test3"]
str1.split(separator: " ", maxSplits: 1) //최대로 자를수 있는 값을 1로 고정해놨기 때문에 한번만 자르고 그다음은 자르지 않는다.["test1", "test2 test3"]
str1.split(separator: " ", omittingEmptySubsequences: false) // "  " false경우 빈 시퀀스를 받겠다. ["test1", "test2", "", "test3"]

//split vs components랑 비교필요해~
let str2 = ["1", "2", "3", "4", "5"]
let str2Joined = str2.joined() //원본은 그대로 있고 문자열을 합쳐준다. 12345
let str2Joined2 = str2.joined(separator: ", ") //요소 사이에 구분기호를 추가할 수 있다(공백도 포함). 1, 2, 3, 4, 5 | 1,2,3,4,5

let str3 = [["1"], ["2"], ["3"], ["4"], ["5"]]
let str3Joined = str3.joined(separator: ["hello", "world"]) //JoinedSequence<Array<Array<String>>>(_base: [["1"], ["2"], ["3"], ["4"], ["5"]], _separator: ContiguousArray(["hello", "world"])) 리턴값 형태가 너무 지저분하네....
```
