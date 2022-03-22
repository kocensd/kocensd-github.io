---
layout: post
title: 해커랭크-makeAnagram(Swift)
tags: [Algorithm]
sitemap :
  changefreq : daily
  priority : 1.0
---

###### 문제 해설

- 아나그램을 만들기 위해 몇개의 문자를 삭제해야하는가?
- 예전에 풀었는데 다시 복습하느라 봤더니 문제가 살짝 변경되어 있네
- a 문자열을 [String: Int]로 변경하고 b문자열을 체크하면서 - 해주고나서 0값이 아닌 문자갯수를 체크해서 더해준다.

```c

func makeAnagram(a: String, b: String) -> Int {
    var dic: [String: Int] = [:]
        _ = a.map{ String($0) }.map { str in
            
            dic[str] = (dic[str] ?? 0) + 1
        }
        
        _ = b.map{ String($0) }.map { str in
            dic[str] = (dic[str] ?? 0) - 1
        }
    let count = dic.filter { $0.value != 0 }.reduce(0, { (res, current) -> Int in
        var res = res
        res += abs(current.value)
        return res
    })
    return count
}

```
