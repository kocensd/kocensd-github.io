---
layout: post
title: iOS 개발자 이직
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---

#### Pod으로 되어있는 라이브러리들을 SPM으로 변경시키기
1. 먼저 사용하고 있는 pod에서 SPM을 지원하는지 체크를 해야한다.
2. 사용하고 있는 버전등을 체크하여 최대한 오류수정을 최소화 한다.
 (버전이 올라가면서 라이브러리등이 많은 변화가 있으면 현재 서비스되고 있는 앱을 업데이트하게 될때 많은 영향이 가지않게 하기 위해서)



#### 실습
이전에 SPM을 테스트 했을때는 따로따로 하나씩 적용했지만 실제 프로젝트에 적용할때는 
개발 버전과 라이브 버전이 다를 수 있기때문에 module를 따로 만들어주도록 한다.
1. xcode에서 프로젝트를 하나 만들어준다. (ex SPMTest 로 만들었음)
2. xcode -> File -> Swift Package
<img src="https://user-images.githubusercontent.com/45751308/114884540-cd09d000-9e40-11eb-9251-3678e7f118a2.png" width="60%">

3. 이름은 기본으로 MyLibrary로 되어있고 현재 프로젝트에 추가를 시켜준다.
<img src="https://user-images.githubusercontent.com/45751308/114884606-dc891900-9e40-11eb-8c42-e204581cb740.png" width="60%">

4. Package.swift 파일에 테스트로 RxSwift의 git주소와 targets의 dependencies에 RxSwift라고 적어준다.
<img src="https://user-images.githubusercontent.com/45751308/114885443-9c766600-9e41-11eb-8f3c-69ccfb97dcf1.png" width="60%">

5. 추가가 된 모습이고 RxSwift가 설치가 되고 있는 모습이다.
<img src="https://user-images.githubusercontent.com/45751308/114884683-ed398f00-9e40-11eb-971a-b788eb85667b.png" width="60%">

6. target -> General의 framework에서 새로만든 package를 추가해준다.
<img src="https://user-images.githubusercontent.com/45751308/114884766-fcb8d800-9e40-11eb-8ad8-f7aea3cc894c.png" width="60%">

7. ViewController.swift파일에서 이제 import RxSwift를 할수있게 된다.
<img src="https://user-images.githubusercontent.com/45751308/114884927-2245e180-9e41-11eb-864b-030f70444ea3.png" width="60%">












