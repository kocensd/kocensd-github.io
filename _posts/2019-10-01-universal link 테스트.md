---
layout: post
title: universal link 간단하게 테스트하기
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---

- ![Image Alt 텍스트](https://user-images.githubusercontent.com/45751308/65927841-57910700-e436-11e9-87dc-59cbeb79d6a3.png)

1. 연결할 웹사이트의 root경로에 해당 파일을 넣어준다
- 웹사이트는 https이여야 하고 파일은 확장자가 있으면 안된다.

```c
{
  "applinks": {
    "apps": [],
    "details": [
    {
      "appID": "팀ID.번들ID",
      "paths": ["*"]
    }
    ]
  }
}
```

2. xocde타겟에서 위의 번들ID의 타겟을 선택하고 Capabilities탭을 선택한다.

3. Associated Domains의 버튼을 On으로 변경해주고, Domains에 주소를 추가해준다.
- applinks:적용할 url (applinks:는 필수로 맨 앞에 들어가줘야 한다.)

4. 빌드 후 메모장에 해당 url을 적은뒤에 클릭하면 바로 앱으로 넘어가는걸 볼 수 있다.
