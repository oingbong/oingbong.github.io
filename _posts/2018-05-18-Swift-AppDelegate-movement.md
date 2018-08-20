---
layout: post
title: AppDelegate Method 실행관련
tags:
- Swift
- AppDelegate
---

### AppDelegate Method
1. application
2. applicationWillResignActive
3. applicationDidEnterBackground
4. applicationWillEnterForeground
5. applicationDidBecomeActive
6. applicationWillTerminate

### 실행 상황
* 앱 최초 실행할때 1 , 5 약간 텀
* 앱 백그라운드 나갈때 2 , 3 약간 텀
* 앱 백그라운드에서 다시 들어올 때 4, 5 동시
* 앱 제어센터 올릴 때 2
* 앱 제어센터 내릴 때 5
* 멀티태스킹 볼 때(다른앱같이보는거) 2
* 멀티태스킹 끌 때(다른앱같이보는거) 5
* 앱안에서 폰 화면 끌 때 2 ,3 동시
* 앱안에서 폰 화면 킬 때 4, 5 동시