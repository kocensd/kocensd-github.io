---
layout: post
title: UIPickerView 커스텀하기
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---

#### UIPickerView Custom하기

완성화면

<img width="20%" src="https://user-images.githubusercontent.com/45751308/158632095-88e0822c-0e71-457b-8bf8-2d8ba9a882dc.gif">

주의할점
1. 해당 피커뷰를 만들때 customView를 추가해주고 배경색을 미리 세팅을 해놓은 상태에서 alpha값을 0으로 세팅해둔다.
2. 해당 셀이 선택될때는 alpha값을 다시 1값으로 세팅을 해주면 git와 같이 자연스럽게 배경색이 변경되는 pickerView를 만들 수 있다.

```c
/// 해당메소드에서 custom뷰를 추가해준다.
func pickerView(_ pickerView: UIPickerView, viewForRow row: Int, forComponent component: Int, reusing view: UIView?) -> UIView {
    ///ex
    let contentView = ContentView(row)
    contentView.unSelected()
    decorateView(pickerView, row: row)

    return contentView
}


func unSelected() {
    self.colorView?.alpha = 0
}

func decorateView(_ picker: UIPickerView, row: Int) {
    for i in 0..<10 {
        if let item = picker.view(forRow: i, forComponent: 0) as? ContentView {
            item.selected(row + 1)
        }
    }
}

```
