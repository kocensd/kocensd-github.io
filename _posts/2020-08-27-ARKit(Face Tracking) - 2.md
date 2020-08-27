---
layout: post
title: Face Tracking - 얼굴에 이모티콘 추가하기
tags: [ARKit]
sitemap :
  changefreq : daily
  priority : 1.0
---

#### ARKit Face Tracking - 얼굴에 이모티콘 추가하기
- 1편에 이어서 이모티콘을 추가해주고 해당 이모티콘도 얼굴을 움직일시에 따라 움직이게 만들어준다.


```c
let noseOptions = ["👃"] // 얼굴위에 놓을 이모티콘을 하나 선언해준다.

func renderer(_ renderer: SCNSceneRenderer, nodeFor anchor: ARAnchor) -> SCNNode? {
    guard let device = sceneView.device else {
        return nil
    }
    let faceGeometry = ARSCNFaceGeometry(device: device)

    let node = SCNNode(geometry: faceGeometry)

    node.geometry?.firstMaterial?.fillMode = .lines

//해당 이모티콘을 세팅하고 새로운 node를 하나 만들어서 추가시켜 준 다음에 기존 노드에 add시켜준다.
    let plane = SCNPlane(width: 0.06, height: 0.06)
    plane.firstMaterial?.diffuse.contents = (noseOptions.first ?? " ").image()
    plane.firstMaterial?.isDoubleSided = true

    let noseNode = SCNNode()
    noseNode.name = "nose"
    noseNode.geometry = plane

    node.addChildNode(noseNode)

    return node
}
```

```c
func renderer(_ renderer: SCNSceneRenderer, willUpdate node: SCNNode, for anchor: ARAnchor) {
    guard let faceAnchor = anchor as? ARFaceAnchor, let faceGeometry = node.geometry as? ARSCNFaceGeometry else {
        return
    }

    //얼굴을 움직일때 이모티콘의 움직임도 함께 업데이트 시켜주기 위해서 사용
    updateFeatures(for: node, using: faceAnchor)

    faceGeometry.update(from: faceAnchor.geometry)
}
```

```c
func updateFeatures(for node: SCNNode, using anchor: ARFaceAnchor) {

    //코 단 한개만 했을때 이렇게 진행했지만 눈코입등을 다 하려면 for문에서 처리해야한다.
    let child = node.childNode(withName: "nose", recursively: false)
    let vertices = [anchor.geometry.vertices[9]] //ARFaceGeometry는 1220개의 정점을 가지고 있고 그중에서 9는 코다.

    let newPos = vertices.reduce(vector_float3(), +) / Float(vertices.count)
    child?.position = SCNVector3(newPos)
}
```
