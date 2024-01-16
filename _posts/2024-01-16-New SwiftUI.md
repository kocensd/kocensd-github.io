---
layout: post
title: 그냥 하는거 작성하기
tags: [SwiftUI]
sitemap :
  changefreq : daily
  priority : 1.0
---

###### 
iOS15 이상 기준
좀 작성좀 하자 한달에 한번씩이라도 ... 구글 검색하기 찾기 귀찮다

    <img width="50%" src="[https://user-images.githubusercontent.com/45751308/206107845-0d86b7e2-d848-44a0-ab77-5009fd29d1e9](https://github.com/kocensd/kocensd.github.io/assets/45751308/709db102-9409-4f3e-a447-63fd1bddfac0).png">
    
```c

struct AlertView: View {
    @State private var isShow = false
    
    var body: some View {
        VStack {
            Text("텍스트")
                .padding() // 위치에 따라 텍스트와 배경색의 패딩
                .background(.green)
                .alert(isPresented: $isShow, content: {
                    let firstButton = Alert.Button.default(Text("확인")) {
                        print("확인버튼 누름")
                    }
                    let secondButton = Alert.Button.cancel(Text("취소")) {
                        print("취소버튼 누름")
                    }
                    return Alert(title: Text("알럿뷰"),
                                 message: Text("!!!"),
                                 primaryButton: firstButton, secondaryButton: secondButton)
                })
                .onTapGesture { //Text에서는 onTapGesture에서 처리
                    self.isShow.toggle()
                }
            
            Button("버튼") { //Button에서는 action에서 처리 
                self.isShow.toggle()
            }
            .foregroundColor(.white)
            .background(.red)
            .padding() // 위치에 따라 Text와의 패딩
            
            .alert(isPresented: $isShow) {
                let firstButton = Alert.Button.default(Text("확인")) {
                    print("확인버튼 누름")
                }
                let secondButton = Alert.Button.cancel(Text("취소")) {
                    print("취소버튼 누름")
                }
                return Alert(title: Text("알럿뷰"),
                             message: Text("!!!"),
                             primaryButton: firstButton, secondaryButton: secondButton)
            }
            
            
        }
    }
}

```
