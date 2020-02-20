---
layout: post
title: Main Thread Checker 오류 
sitemap :
  changefreq : daily
  priority : 1.0
---

- 갑자기 어느날 빌드를 돌려보니 이런 로그가 찍히면서 앱이 동작하는게 이상했다.

```c
Main Thread Checker: UI API called on a background thread: -[UIView setHidden:]
PID: 2542, TID: 117015, Thread name: (none), Queue name:
~~~~

```

- 검색을 해보니 UI를 MainThread에서 처리를 안해줘서 그렇다고 한다.
- ?? 난 제대로 한거같은데... 하다가 찾은게 
- 푸시구현을 하고 앱 첫 실행시 권한설정이 뜨는데 이게 문제가 된것이다.(안그랬느데...)

```c
//이부분쪽에 MainThread동작을 해주면 된다. 

DispatchQueue.main.async {
  UIApplication.shared.registerForRemoteNotifications()
}
```
