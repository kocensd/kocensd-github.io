---
layout: post
title: 해커랭크-Special String Again(Swift)
tags: [Algorithm]
sitemap :
  changefreq : daily
  priority : 1.0
---



###### 문제 해설

- 특별한 단어찾기 aaa 모든 문자가 같던지 aabaa 가운데 글자만 빼고 나머지 문자가 같은지


실패
- 회문을 돌려서 비교값이 서로 다르면 앞뒤 문자열을 비교한다. 아이패드 산기념으로 
<img width="20%" src="https://user-images.githubusercontent.com/45751308/159646719-8bb2a042-ef7b-45e2-ba02-d74af1cca155.jpg">

```c
func substrCount(n: Int, s: String) -> Int {

    let arr = s.map { String($0) }
    var count = 0

    for i in 0..<arr.count {
        for j in i+1..<arr.count {
            if arr[i] != arr[j] {
                if ((j+1)+(j-i) <= arr.count) && (arr[i..<j].joined() == arr[j+1..<(j+1)+(j-i)].joined()) {
                    count += 1
                    break
                } else {
                    break
                }
            } else {
                count += 1
            }
        }
    }
    return count + arr.count
}
```

해설문 참고
<img width="20%" src="https://user-images.githubusercontent.com/45751308/159646726-ecb1d9c1-1331-436a-bafd-63fb10e63b95.jpg">

```c
func substrCount(n: Int, s: String) -> Int {
    var countTable = [(char: Character, count: Int)]()

      var lastChar: Character = "0"
      var charCount = 0
      for char in s {
        if char == lastChar {
          charCount += 1
        } else {
          if lastChar != Character("0") {
            countTable.append((lastChar, charCount))
          }
          lastChar = char
          charCount = 1
        }
      }
      countTable.append((lastChar, charCount))
    
    print("countTable: \(countTable)")

      var paliCount = 0
      for (index, charSet) in countTable.enumerated() {

        let count = charSet.count
        paliCount += count*(count+1)/2
          print("palicount: \(paliCount)")
        if index >= 2 {
          if countTable[index-1].count == 1, countTable[index-2].char == charSet.char {
            paliCount += min(countTable[index-2].count, count)
          }
        }
      }
      return paliCount
}

```
