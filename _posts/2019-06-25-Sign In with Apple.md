---
layout: post
title: '[기술 면접 질문] 기술 면접 예상 질문 대비하기 - 스프링(Spring)편'
subtitle: '기술 면접 예상 질문 대비하기 - 스프링(Spring)편'
date: 2017-10-01
author: heejeong Kwon
cover: '/images/basic-concepts-of-development/basic-concepts-of-development-main.png'
tags: 면접 기본개념 스프링 spring
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

1. ViewController

```c
import UIKit
import AuthenticationServices

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        setupView()
    }
    
    func setupView() {
        let appleButton = ASAuthorizationAppleIDButton()  // 버튼 생성
        appleButton.translatesAutoresizingMaskIntoConstraints = false
        appleButton.addTarget(self, action: #selector(didTapAppleButton), for: .touchUpInside)
        
        view.addSubview(appleButton)
        NSLayoutConstraint.activate([
            appleButton.centerYAnchor.constraint(equalTo: view.centerYAnchor),
            appleButton.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 30),
            appleButton.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -30)
            ])
    }
    
    @objc
    func didTapAppleButton() {
        let provider = ASAuthorizationAppleIDProvider()
        let request = provider.createRequest()
        request.requestedScopes = [.fullName, .email]
        
        let controller = ASAuthorizationController(authorizationRequests: [request])
        controller.delegate = self
        controller.presentationContextProvider = self
        controller.performRequests()
    }

}
extension ViewController: ASAuthorizationControllerDelegate {
    func authorizationController(controller: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization) {
        switch authorization.credential {
        case let credentials as ASAuthorizationAppleIDCredential:
            let user = User(credentials: credentials)
            let vc = UIStoryboard(name: "Main", bundle: nil).instantiateViewController(withIdentifier: "MainViewController") as! MainViewController
            vc.user = user
            self.present(vc, animated: true, completion: nil)
        default:
            break
        }
    }
    
    func authorizationController(controller: ASAuthorizationController, didCompleteWithError error: Error) {
        print("something bad happened", error)
    }
}

extension ViewController: ASAuthorizationControllerPresentationContextProviding {
    func presentationAnchor(for controller: ASAuthorizationController) -> ASPresentationAnchor {
        return view.window!
    }
}
```

```c
import UIKit

class MainViewController: UIViewController {

    @IBOutlet weak var detailsLabel: UILabel!
    
    var user: User?
    
    override func viewDidLoad() {
        super.viewDidLoad()

        detailsLabel.text = user?.debugDescription
    }
}
```

```c
import Foundation
import AuthenticationServices

struct User {
    let id: String
    let firstName: String
    let lastName: String
    let email: String
    
    init(credentials: ASAuthorizationAppleIDCredential) {
        self.id = credentials.user
        self.firstName = credentials.fullName?.givenName ?? ""
        self.lastName = credentials.fullName?.familyName ?? ""
        self.email = credentials.email ?? ""
    }
    
}

extension User: CustomDebugStringConvertible {
    var debugDescription: String {
        return """
        ID: \(id)
        First Name: \(firstName)
        Last Name: \(lastName)
        Email: \(email)
        """
    }
}
```



