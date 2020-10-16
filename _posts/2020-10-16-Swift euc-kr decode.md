---
layout: post
title: Swift euc-kr decode
tags: [Swift]
sitemap :
  changefreq : daily
  priority : 1.0
---

#### Swift - euc-kr 디코딩하기

### 상품클릭이나 결제등을 마친 후에 해당 url에 값을 가져올 경우가 있다. 그런데 utf-8이 아니라 euc-kr인코딩이 되어있는 값을 가져올때 처리방법
- %B9%D9%C1%F6  -> euc-kr로 디코딩하면 "바지" 값이 나온다. 이걸 나오게 하는 방법은?

```c
extension String.Encoding {
    static let eucKrDecode = String.Encoding(rawValue: CFStringConvertEncodingToNSStringEncoding(0x0422))
}

extension String {
    func bytesByRemovingPercentEncoding(using encoding: String.Encoding) -> Data {
        struct My {
            static let regex = try! NSRegularExpression(pattern: "(%[0-9A-F]{2})|(.)", options: .caseInsensitive)
        }
        var bytes = Data()
        let nsSelf = self as NSString
        for match in My.regex.matches(in: self, range: NSRange(0..<self.utf16.count)) {
            if match.range(at: 1).location != NSNotFound {
                let hexString = nsSelf.substring(with: NSMakeRange(match.range(at: 1).location+1, 2))
                bytes.append(UInt8(hexString, radix: 16)!)
            } else {
                let singleChar = nsSelf.substring(with: match.range(at: 2))
                bytes.append(singleChar.data(using: encoding) ?? "?".data(using: .ascii)!)
            }
        }
        return bytes
    }
    func removingPercentEncoding(using encoding: String.Encoding) -> String? {
        return String(data: bytesByRemovingPercentEncoding(using: encoding), encoding: encoding)
    }
}

//단순 문자일때
let product = "%B9%D9%C1%F6"
if let str = product.removingPercentEncoding(using: .eucKrDecode) {
    print(str) // 바지            
}

//[key: value] 형태일때
func decodeJsonData(_ str: String) -> Any {
    var result: Any = ""
    do{
        if let json = str.data(using: String.Encoding.utf8){
            result = try JSONSerialization.jsonObject(with: json, options: [])
        }
    }catch {
        print(error.localizedDescription)
    }
    return result
}

let products = "%5B%7B%22productName%22%3A%22%C5%D7%BD%BA%C6%AE1%22%2C%22unitPrice%22%3A%2224000%22%2C%22quantity%22%3A%221%22%7D%2C%7B%22productName%22%3A%22%C5%D7%BD%BA%C6%AE2%22%2C%22unitPrice%22%3A%220%22%2C%22quantity%22%3A%221%22%7D%5D"
// [["unitPrice": 24000, "quantity": 1, "productName": 테스트1], ["productName": 테스트2, "unitPrice": 0, "quantity": 1]]

if let str = products.removingPercentEncoding(using: .eucKrDecode) {
    let decodeStr = decodeJsonData(str) 
    if let jsonData = decodeStr as? [[String: Any]] {
        print(jsonData)
    }
}
            
```
