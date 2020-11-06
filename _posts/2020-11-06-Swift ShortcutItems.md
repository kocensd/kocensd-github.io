---
layout: post
title: Swift ShortcutItems
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---


#### Swift - 숏컷 아이템 만들기(static vs dynamic) - 앱을 길게 눌렀을때 나오는 메뉴들을 만들어보자

### 1. static형식으로 만들기
1. plist에 추가해주기

```c
<key>UIApplicationShortcutItems</key>
	<array>
		<dict>
			<key>UIApplicationShortcutItemIconType</key>
			<string>UIApplicationShortcutIconTypeCompose</string> //아이콘
			<key>UIApplicationShortcutItemTitle</key>
			<string>간편배송조회</string> //메뉴에 나오는 title
			<key>UIApplicationShortcutItemType</key>
			<string>quickDelivery</string> // code에서 어떤메뉴를 클릭했는지 확인하는 구분값
			<key>UIApplicationShortcutItemUserInfo</key>
			<dict>
				<key>key1</key>
				<string>value2</string>
			</dict>
		</dict>
		<dict>
			<key>UIApplicationShortcutItemIconType</key>
			<string>UIApplicationShortcutIconTypeCompose</string>
			<key>UIApplicationShortcutItemTitle</key>
			<string>마이페이지</string>
			<key>UIApplicationShortcutItemType</key>
			<string>quickMypage</string>
			<key>UIApplicationShortcutItemUserInfo</key>
			<dict>
				<key>key2</key>
				<string>value2</string>
			</dict>
		</dict>
  </array>
```

2. Appdelegate.swift에서 메뉴 선택시 관련 함수를 작성해준다. (생각보다 간단하다) //여기서 앱이 background 상태인지 kill 상태인지 구분헤서 처리해주는것이 좋다.

```c
func application(_ application: UIApplication, performActionFor shortcutItem: UIApplicationShortcutItem, completionHandler: @escaping (Bool) -> Void) {        
        guard let actionType = QuickAction(rawValue: shortcutItem.type) else {
            return
        }
        switch (actionType) {
        case .quickDelivery:
            print("quickDelivery") //화면을 이동하거나 이벤트를 발생시키는 부분은 이 영역에서 처리해주면 된다.
        case .quickMypage:
            print("quickMypage")
        }
    }
```

3. 적용해야할 target이 많다고 하면 plist에 shellScript를 이용하여 추가해줄 수 있다.
```c
local project_path=프로젝트 파일 위치
local newPlist=${project_path}/Info.plist
/usr/libexec/PlistBuddy -c "Add UIApplicationShortcutItems array" "${newPlist}"
/usr/libexec/PlistBuddy -c "Set UIApplicationShortcutItems:0 dict" "${newPlist}"
/usr/libexec/PlistBuddy -c "Add UIApplicationShortcutItems:0:UIApplicationShortcutItemIconType string UIApplicationShortcutIconTypeCompose" "${newPlist}"
/usr/libexec/PlistBuddy -c "Add UIApplicationShortcutItems:0:UIApplicationShortcutItemTitle string 간편배송조회" "${newPlist}"
/usr/libexec/PlistBuddy -c "Add UIApplicationShortcutItems:0:UIApplicationShortcutItemType string quickDelivery" "${newPlist}"
/usr/libexec/PlistBuddy -c "Add UIApplicationShortcutItems:0:UIApplicationShortcutItemUserInfo:key4 string value4" "${newPlist}"
```

### 2. dynamic형식으로 만들기
- 서버에서 내려오는 데이터에 따라서 숏컷 아이템을 구성해야한다면? 그렇다면 굳이 plist에 추가해주지 않고 코드로 작성해주면된다.
- Appdelegate에 작성된 호출 함수는 그대로 두고 서버에서 데이터를 받아온 뒤 MainViewController에서 숏컷 아이템을 넣어준다

```c
let icon1 = UIApplicationShortcutIcon.init(templateImageName: "menu_delivery") //아이콘 이름
let icon2 = UIApplicationShortcutIcon.init(templateImageName: "menu_page")

let item1 = UIMutableApplicationShortcutItem.init(type: "quickDelivery", localizedTitle: "간편배송조회", localizedSubtitle: "", icon: icon1, userInfo: nil)
let item2 = UIMutableApplicationShortcutItem.init(type: "quickMypage", localizedTitle: "마이페이지", localizedSubtitle: "", icon: icon2, userInfo: nil)

let items = [item1, item2] as Array
let updatedItems: Array = items

UIApplication.shared.shortcutItems = updatedItems
```











