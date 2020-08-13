---
layout: post
title: Face Tracking - 얼굴위치 및 방향추적
tags: [ARKit]
sitemap :
  changefreq : daily
  priority : 1.0
---

## ARKit Face Tracking - 얼굴 추적
- ARFaceTrackingConfiguration을 사용하여 얼굴을 추적해보자

###### 기본세팅
1. ARSCNView만들어주고 ARKit를 import해준다.
2. ARSCNView의 delegate를 연결해준다.
3. ARSCNView의 session을 시작해준다.

###### 얼굴 감지 delegate source
- ARKit은 사용자 얼굴의 크기, 모양, 토폴로지 및 현재 얼굴 표정과 일치하는 거친 3D 메시 지오메트리를 제공합니다. 
- ARSCNFaceGeometry: ARKit은 또한 SceneKit 메시를 시각화하는 쉬운 방법을 제공하는 클래스를 제공합니다.

```c
//ARSCNViewDelegate
extension FaceTrackingBasicViewController: ARSCNViewDelegate {
    func renderer(_ renderer: SCNSceneRenderer, nodeFor anchor: ARAnchor) -> SCNNode? {
        guard let device = sceneView.device else {
            return nil
        }
        let faceGeometry = ARSCNFaceGeometry(device: device) //시각화
        
        let node = SCNNode(geometry: faceGeometry) //node에 faceGeometry를 넣어준다.

        node.geometry?.firstMaterial?.fillMode = .lines //face에 line을 그려줄것인가
        return node
    }
    
    //눈을 깜박거리거나, 입을 벌리거나, 얼굴을 움직일때마다 계속 업데이트 시켜준다.
    func renderer(_ renderer: SCNSceneRenderer, willUpdate node: SCNNode, for anchor: ARAnchor) {
        guard let faceAnchor = anchor as? ARFaceAnchor, let faceGeometry = node.geometry as? ARSCNFaceGeometry else {
            return
        }
        
        faceGeometry.update(from: faceAnchor.geometry)
    }
}
```
