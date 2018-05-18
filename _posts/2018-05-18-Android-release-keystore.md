---
layout: post
title: 안드로이드 릴리즈용 키스토어 만들기
tags:
- Android
- Keystore
---


개념

1.키스토어 생성 : 키스토어 파일은 개인 키를 모아둔 하나의 저장공간
2.개인키 생성 : 실제 개발한 앱을 소유한 사람(또는 단체)이 누구인지 식별해주는 키

* 한번 배포한 이후 다음 업데이트부터는 반드시 처음 배포했던 APK에 서명한 바로 그 키로 서명해야 합니다.

만약에 해당 키스토어 파일을 잃어 버리거나 키스토어 비밀번호, 개인키 비밀번호 등을 잊어버리면 다시는 그 앱을 업데이트 할 수 없습니다.
새로 올리게 되면 , 서명이 다르므로 다른 앱으로 인식합니다.


작업

1.인증서 만들기 : keytool 을 이용하여 터미널에서 수동으로 생성

예시
$ keytool -genkey -v -keystore release_key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000

release_key.keystore : 키스토어 파일 이름 임의 지정
alias_name : 앱을 키스토어 파일로 서명할 때 사용할 이름
10000 : 유효기간 ( 유효기간이 지나면 더이상 해당앱은 업데이트 할 수 없음 )

실제 (유효기간 : 2117년 11월 18일)
$ keytool -genkey -v -keystore release_key.keystore -alias release_key -keyalg RSA -keysize 2048 -validity 36500

키 저장소 비밀번호 입력 : password123
새 비밀번호 다시 입력 : password123
이름과 성을 입력하십시오. : OINGBONG
조직 단위 이름을 입력하십시오. : OINGBONG
조직 이름을 입력하십시오. : OINGBONG
구/군/시 이름을 입력하십시오? : Gangnam-gu
시/도 이름을 입력하십시오. : Seoul
이 조직의 두 자리 국가 코드를 입력하십시오 : kr
CN=OINGBONG, OU=OINGBONG, O=OINGBONG, L=Gangnam-gu, ST=Seoul, C=kr 이(가) 맞습니까? y

다음에 대해 유효기간이 36,500일인 2,048비트 RSA 키 쌍 및 자체 서명된 인증서 (SHA256withRSA)를 생성하는중
 : CN=OINGBONG, OU=OINGBONG, O=OINGBONG, L=Gangnam-gu, ST=Seoul, C=kr

<relesase_key>에 대한 키 비밀번호를 입력하십시오.
(키 저장소 비밀번호와 동일한 경우 Enter 키를 누름) : 

키스토어 파일 생성완료 ls


2.릴리즈 빌드 및 APK 생성

안드로이드 스튜디오 - Build - Generate Signed APK…
Module : app 선택되어있음 - Next 버튼 
Key store path : Choose existing… 버튼을 눌러 경로 및 키스토어 파일을 선택
Key store password : 비밀번호 입력
Key alias : 옆에 … 버튼을 누르고 Use an existing Key : 에 선택된 release_key 선택 후 OK
Key password : 비밀번호 입력

Key store path : /Users/oingbong/Documents/TESTAPP/keyList/release_key.keystore
Key store password : password123
Key alias : release_key
Key password : password123

입력 후 Next

APK Destination Folder : 릴리즈 빌드를 하고 서명할 APK 파일이 생성될 폴더의 위치 선택
Build Type : release 선택

APK Destination Folder : /Users/oingbong/Documents/TESTAPP/keyList
Build Type : release 

선택 후 Finish - APK 릴리즈 빌드와 서명 시작 - 완료 후 메세지 뜸
Generate Signed APK
APK(s) generated successfully.

파일 생성된 폴더 확인 후 앱 배포파일 준비완료.


3.릴리즈 빌드 및 APK 생성시 에러 나는 경우
alias 선택을 다시 해주니 잘 됩니다…………..


4.앱 업데이트 할 때 주의사항
Project 보기 - app - build.gradle
android - defaultConfig 안에 두가지 버전업 해줘야 합니다.
최초 출시 버전
versionCode 2
versionName “1.0.0”
다음 출시 버전 예상
versionCode 3
versionName “1.0.1” or “1.1.0"


참고
http://wingsnote.com/84
http://wingsnote.com/85