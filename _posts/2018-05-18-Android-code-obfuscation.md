---
layout: post
title: 안드로이드 코드 난독화 설정
tags:
- Android
---

> 코드 난독화 설정 후 릴리즈 추출을 할 때 오픈소스 사용시 에러 발생합니다.

### 1. 코드 난독화 설정 방법
`Project 보기 - app - build.gradle`
`android - buildTypes - release - minifyEnabled false -> true 로 변경해줍니다.`

### 2. okhttp / retrofit 사용한 경우 코드 난독화 및 릴리즈 추출 에러 뜨는 경우(proguard 관련)
`Project 보기 - app - proguard-rules.pro 에 아래 내용 써줍니다.`

- dontwarn okhttp3.**
- dontwarn okio.**
- dontwarn java.annotation.**

- dontwarn retrofit2.Platform$Java8

#### 참고
http://square.github.io/retrofit/
https://github.com/square/okhttp