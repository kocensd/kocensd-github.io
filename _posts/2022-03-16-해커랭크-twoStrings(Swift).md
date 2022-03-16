---
layout: post
title: 해커랭크-twoStirngs(Swift)
tags: [Algorithm]
sitemap :
  changefreq : daily
  priority : 1.0
---


###### 문제 해설

- 중복된 단어가 있는지 체크하기
- 평소에 Set을 잘 사용하는 부분이 없었는데 Set을 이용하여 푼것때문에 작성
- string을 map으로 쪼개서 set배열에 넣고 각자 count보다 합친거의 count가 작으면 중복된 단어가 존재한다고 체크함. 중복된 값을 체크하지 않는 이상 count로만 체크해도됨

```c
import Foundation

func twoStrings(s1: String, s2: String) -> String {
    
    let s1Set = Set<String>(s1.map { String($0) })
    let s2Set = Set<String>(s2.map { String($0) })
    let sumCount = s1Set.count + s2Set.count
    let totalCount = s1Set.union(s2Set).count
    
    return sumCount > totalCount ? "YES" : "NO"
}

```

print(twoStrings(s1: "hello", s2: "world"))
print(twoStrings(s1: "hi", s2: "world"))
