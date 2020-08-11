---
layout: post
title: ARKit(Plane, 수평면)
tags: [ARKit]
sitemap :
  changefreq : daily
  priority : 1.0
---

## ARKit Plane - 수평면
- configuration.planeDetection = .horizontal // 수평면 감지
- sceneView.debugOptions = [ARSCNDebugOptions.showFeaturePoints] //수평면 점 효과

###### 감지된 수평면을 시각화 하기

```c
//수평면 시각화 - 수평면을 인식하게 되면 회색색상의 노드가 화면에 나온다.
func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
    guard let planeAnchor = anchor as? ARPlaneAnchor else { return }

     let width = CGFloat(planeAnchor.extent.x)
     let height = CGFloat(planeAnchor.extent.z)
     let plane = SCNPlane(width: width, height: height)
    plane.materials.first?.diffuse.contents = UIColor.lightGray

     let planeNode = SCNNode(geometry: plane)
     let x = CGFloat(planeAnchor.center.x)
     let y = CGFloat(planeAnchor.center.y)
     let z = CGFloat(planeAnchor.center.z)
     planeNode.position = SCNVector3(x,y,z)
     planeNode.eulerAngles.x = -.pi / 2

     node.addChildNode(planeNode)
}
```

###### 감지된 수평면을 확장하기

```c
func renderer(_ renderer: SCNSceneRenderer, didUpdate node: SCNNode, for anchor: ARAnchor) {
  //수평면 확장 - 카메라가 수평면을 더 많이 감지하면 그에 따라서 노드도 확장된다.
  guard let planeAnchor = anchor as?  ARPlaneAnchor,
      let planeNode = node.childNodes.first,
      let plane = planeNode.geometry as? SCNPlane else { return }

  let width = CGFloat(planeAnchor.extent.x)
  let height = CGFloat(planeAnchor.extent.z)
  plane.width = width
  plane.height = height

  let x = CGFloat(planeAnchor.center.x)
  let y = CGFloat(planeAnchor.center.y)
  let z = CGFloat(planeAnchor.center.z)
  planeNode.position = SCNVector3(x, y, z)
}
```

###### 수평면 위에 object올리기

```c
//먼저 tapGesture()로 이벤트를 발생시킨 뒤 수평면 노드일 경우 박스를 올려준다.
//1
sceneView.rx
    .tapGesture()
    .when(.recognized)
    .subscribe(onNext: { gesture in
        self.setObject(gesture)
}).disposed(by: disposeBag)

//2
func setObject(_ recognizer : UIGestureRecognizer) {
    let sceneView = recognizer.view as! ARSCNView
    let touchLocation = recognizer.location(in: sceneView)
    let hitTestResult = sceneView.hitTest(touchLocation, types: .existingPlaneUsingExtent)
    if !hitTestResult.isEmpty {
        guard let hitResult = hitTestResult.first else {
            return
        }
        addBox(hitResult :hitResult)
    }
}

//3
func addBox(hitResult: ARHitTestResult) {
    let boxGeometry = SCNBox(width: 0.2, height: 0.2, length: 0.1, chamferRadius: 0)
    let material = SCNMaterial()
    material.diffuse.contents = UIColor.red
    boxGeometry.materials = [material]
    let boxNode = SCNNode(geometry: boxGeometry)

    boxNode.position = SCNVector3(hitResult.worldTransform.columns.3.x,hitResult.worldTransform.columns.3.y + Float(boxGeometry.height/2), hitResult.worldTransform.columns.3.z)

    self.sceneView.scene.rootNode.addChildNode(boxNode)
}

```












