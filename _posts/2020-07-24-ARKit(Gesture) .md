---
layout: post
title: ARKit Gesture사용하기
tags: [ARKit]
sitemap :
  changefreq : daily
  priority : 1.0
---

## ARKit Gesture사용하기
- 기본 UITapGestureRecognizer사용하여 제스쳐를 감지할 수 있지만 RxGesture를 사용하여 Gesture를 감지해본다.
- SMP(https://github.com/kocensd/kocensd.github.io/blob/master/_posts/2020-06-23-Swift%20Package%20Manager.md)를 사용하여 RxGesture, RxSwift를 설치한다.

###### tap Gesture를 사용하여 box 색상변경하기
```c
sceneView.rx
  .tapGesture()
  .when(.recognized)
  .subscribe(onNext: { gesture in
      self.tapGesture(gesture)
  }).disposed(by: disposeBag)
  
func tapGesture (_ recognizer: UIGestureRecognizer) {
    let location = recognizer.location(in: sceneView)
    let results = sceneView.hitTest(location, options: nil)

    if let result = results.first {
        let boxColor = result.node.geometry?.material(named: "Color")
        boxColor?.diffuse.contents = getRandomColor()
    }
}

func getRandomColor() -> UIColor{
    let randomRed:CGFloat = CGFloat(drand48())
    let randomGreen:CGFloat = CGFloat(drand48())
    let randomBlue:CGFloat = CGFloat(drand48())
    return UIColor(red: randomRed, green: randomGreen, blue: randomBlue, alpha: 1.0)
}
```

###### pan Gesture
```c
sceneView.rx
  .panGesture()
  .when(.began, .changed, .ended)
  .subscribe(onNext: { gesture in
      self.panGesture(gesture)
  }).disposed(by: disposeBag)
  
func panGesture(_ recognizer: UIGestureRecognizer) {
    print("move")
}
```

###### swipe
```c
sceneView.rx
  .swipeGesture([.up, .down, .left, .right])
  .subscribe(onNext: { gesture in
      switch gesture.direction {
      case .up:
          self.swipeGesture(gesture, "up")
      case .down:
          self.swipeGesture(gesture, "down")
      case .left:
          self.swipeGesture(gesture, "left")
      default:
          self.swipeGesture(gesture, "right")
      }

  }).disposed(by: disposeBag)
  

func swipeGesture(_ recognizer: UIGestureRecognizer, _ direction: String) {
    print(direction)
}
```








