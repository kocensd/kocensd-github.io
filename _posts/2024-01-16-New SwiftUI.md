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

- alert
- confirmationDialog

<img width="30%" src="https://github.com/kocensd/kocensd.github.io/assets/45751308/709db102-9409-4f3e-a447-63fd1bddfac0.png">

```c

struct AlertView: View {
    
    @State private var isShow = false
    @State private var isShow2 = false
    
    var body: some View {
        VStack {
            
            Button("버튼") {
                self.isShow.toggle()
            }
            .foregroundColor(.white)
            .background(.red)
            .padding()
            .confirmationDialog(
                Text("!!"),
                isPresented: $isShow,
                actions: {
                    Button("카메라") {
                        print("카메라 선택")
                    }
                    Button("라이브러리") {
                        print("라이브러리 선택")
                    }
                })
            
            Text("텍스트!!!!!!")
                .padding()
                .background(.green)
                .alert(isPresented: $isShow2, content: {
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
                .onTapGesture {
                    self.isShow2.toggle()
                }
        }
    }
}

```
