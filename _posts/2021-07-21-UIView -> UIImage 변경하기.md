---
layout: post
title: UIView -> UIImage로 변경시키기
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---


#### 앱에서 나타나느 화면으 공유하고 싶을때
1. view를 image로 변경하여 공유를 진행하여야 한다.
2. 카카오톡을 공유를 하고 싶을때는 image를 이미지서버 or 카카오톡에서 제공하는 이미지 업로드 서버를 통해서 url로 공유를 진행하여야 한다.

```c

func viewChangeImg() -> UIImage? {
        
    if #available(iOS 10.0, *) {

        let renderer = UIGraphicsImageRenderer(bounds: self.bounds)

        return renderer.image { rendererContext in

            layer.render(in: rendererContext.cgContext)
        }
    } else {

        UIGraphicsBeginImageContext(self.bounds.size)

        self.layer.render(in: UIGraphicsGetCurrentContext()!)

        if let image = UIGraphicsGetImageFromCurrentImageContext() {

            UIGraphicsEndImageContext()

            let rtnImg = image.resizedImage(self.bounds.size)
            return rtnImg
        }

        return nil
    }
}

let img = view.viewChangeImg()

```
