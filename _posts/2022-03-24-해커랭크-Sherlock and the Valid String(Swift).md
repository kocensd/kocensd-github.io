---
layout: post
title: 해커랭크-Sherlock and the Valid String(Swift)
tags: [Algorithm]
sitemap :
  changefreq : daily
  priority : 1.0
---

###### 문제 해설

- 해당 문자를 한개 지웠을때 문자열의 모든문자 count가 같아야한다.
- [String: Int] 형태로 변경한 뒤에 다시 count를 가지고 [Int: Int] 형태로 만들어주는게 핵심

- 방법
<img width="40%" src="https://user-images.githubusercontent.com/45751308/159745241-bfc128cc-5b56-4221-8c77-9ec73ea71f1b.jpg">

```c
func isValid(s: String) -> String {
    let arr = s.map{ String($0) }
    
    let dic = arr.reduce([:], { (res, current) -> [String: Int] in
        var res = res
        res[current] = (res[current] ?? 0) + 1
        return res
    })
    
    let resultDic = dic.values.reduce([:], { (res, current) -> [Int: Int] in
        var res = res
        res[current] = (res[current] ?? 0) + 1
        return res
    })
    
    if resultDic.values.count >= 3 {
        return "NO"
    } else if resultDic.values.count == 1 {
        return "YES"
    } else {
        let min = resultDic.values.min()!
        let dis = resultDic.keys.reduce(0) { abs($0 - $1) }
        
        if min == 1 && dis == 1 {
            return "YES"
        } else {
            if resultDic[1] == 1 {
                return "YES"
            } else {
                return "NO"
            }
        }
    }
}
```
