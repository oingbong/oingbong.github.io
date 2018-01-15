---
layout: post
title: iOS11 녹화기능 감지하기
tags:
- Swift
---

### iOS11 녹화기능 감지
iOS11의 녹화기능에 대해 보안이 필요한 경우가 생겼으며 별도의 DRM을 사용하지 않고 간단한 방법으로 감지하는 차선책을 찾게 되었습니다.
++아래 방법 및 설명이 정확한 정보가 아닐수도 있으며 개인적인 테스트 이후에 작성하는 글입니다.++
++제가 작성하는 글보다 더 좋은 방법이 있을수도 있으니 참고해주시면 감사하겠습니다.++

##### 방법
- [isCaptured](https://developer.apple.com/documentation/uikit/uiscreen/2921651-iscaptured) 를 사용하여 메소드 이용
- AppDelegate 에서 앱의 상태에 따라 실행되는 delegate 함수 사용

##### 감지 Method
- iOS 버전 체크 후에 isCaptured가 true 인 경우에 그에 대한 정보를 서버에 전송하거나 기타 작업을 진행합니다.
```
func recordingLog(){
    if #available(iOS 11.0, *) {
        let isCaptured = UIScreen.main.isCaptured

        if(isCaptured){
            // 녹화에 대한 사용자 정보 서버 전송 OR 기타 작업
        }
    } else {
        // Fallback on earlier versions
    }
```

##### delegate 함수 적용
- 코드 아래의 실행되는 예시를 고려하여 적절한 delegate 함수에 사용하였습니다.
```
func applicationWillResignActive(_ application: UIApplication) {
    recordingLog()
}
```
```
func applicationDidBecomeActive(_ application: UIApplication) {
    recordingLog()
}
```

##### Delegate 함수 관련
1. application
2. applicationWillResignActive
3. applicationDidEnterBackground
4. applicationWillEnterForeground
5. applicationDidBecomeActive
6. applicationWillTerminate

##### 실행되는 예시
- 앱 최초 실행할때 1 , 5 약간 텀
- 앱 백그라운드 나갈때 2 , 3 약간 텀
- 앱 백그라운드에서 다시 들어올 때 4, 5 동시
- 앱 제어센터 올릴 때 2
- 앱 제어센터 내릴 때 5
- 멀티태스킹 볼 때(다른앱 같이보는것) 2
- 멀티태스킹 끌 때(다른앱 같이보는것) 5
- 앱안에서 폰 화면 끌 때 2 ,3 동시
- 앱안에서 폰 화면 킬 때 4, 5 동시


