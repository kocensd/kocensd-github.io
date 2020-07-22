---
layout: post
title: Swift Package Manager
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---

- SPM? 이 뭘까 하다가 검색해보니 Swift Package Manager의 약자였습니다.
- 대부분의 개발자들이 CocoaPods로 third-party를 사용하고 있지만 xcode에서 사용이 가능하도록 업데이트(?) 된 것이라고 합니다.
- xcode11부터 기능이 통합됬다고 하는데 너무 늦은감이 있지만 나중에 적용할 수 있기 때문에 작성해둠

- RxSwift를 추가해보기

- File -> Swift Packages -> Add Package Dependency 

<img src="https://user-images.githubusercontent.com/45751308/85355072-d4c15680-b546-11ea-884b-e40a50615f2e.png" width="60%"></img>


- RxSwift gitUrl을 적어준다 
- <img src="https://user-images.githubusercontent.com/45751308/85355105-e276dc00-b546-11ea-9ee0-738a8f1f7018.png" width="60%"></img>

- 설치에 필요한 버전과 브랜치, commit버전으로도 설치가 가능한것 같은데 일단 version으로 진행
- <img src="https://user-images.githubusercontent.com/45751308/85355110-e3a80900-b546-11ea-9fe7-1aca8b45073d.png" width="60%"></img>

- RxBlocking은 사용해보지 않아서 나머지들꺼 체크하고 Finsh
- <img src="https://user-images.githubusercontent.com/45751308/85355116-e571cc80-b546-11ea-9bc8-ec195800ee02.png" width="60%"></img>

- 그렇게 하면 프로젝트 아래에 Swift Package Dependencies라고 생기면서 RxSwift가 나타난다.
- <img src="https://user-images.githubusercontent.com/45751308/85355132-e7d42680-b546-11ea-9b2a-9852c37632c8.png" width="60%"></img>

- RxSwift를 import해주는데 바로 적용이 안되니까 command + B 해주고나면 아래와 같이 활성화 된다.
- <img src="https://user-images.githubusercontent.com/45751308/85355120-e6a2f980-b546-11ea-9ac5-bc937c9180ec.png" width="60%"></img>
- <img src="https://user-images.githubusercontent.com/45751308/85355135-e9055380-b546-11ea-9547-46879c8b7758.png" width="60%"></img>

- 정말되는지 test코드를 작성해본다.
- <img src="https://user-images.githubusercontent.com/45751308/85355140-ea368080-b546-11ea-9768-4c4db666751c.png" width="60%"></img>

- 에러발생......
- <img src="https://user-images.githubusercontent.com/45751308/85355142-eb67ad80-b546-11ea-8688-e56d76ce482e.png" width="60%"></img>

- RxTest가 문제였다. General에 가서 RxTest삭제 (Unit Test에서는 RxTest를 사용하기 때문에 방법을 좀더 확인해봐야할것 같음)
- <img src="https://user-images.githubusercontent.com/45751308/85355147-ec98da80-b546-11ea-9b9d-7b0ebf5e206f.png" width="60%"></img>










