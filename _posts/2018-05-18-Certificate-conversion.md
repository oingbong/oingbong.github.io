---
layout: post
title: 인증서 p12 -> pem 변환 방법 (바이두 - 푸시서비스에 사용)
tags:
- Certificate
- Baidu
- Pem
- P12
---

## 인증서 p12 -> pem 변환 방법 (바이두 - 푸시서비스에 사용)

- 바이두에 푸시서비스 이용하기 위해서는 pem 변환파일이 필요하며 firebase 를 이용할때에는 p12 파일 그냥 올려도 됨

##### 준비
0. 고객에게 받은 aps.cer 파일 더블클릭하여 키체인에 등록하기
1. 키체인 접근 메뉴 열기
2. 해당 인증서와 그 안에 개인 키를 각각 보내기
    이름 예) 인증서 : cert.p12 / 개인키 : key.p12

##### OpenSSL 작업
3. Terminal 열기
4. p12 파일이 있는 폴더 이동
5. cert.pem 생성
    openssl pkcs12 -clcerts -nokeys -out cert.pem -in cert.p12
    (p12 파일 내보낼 때 비밀번호를 지정했다면 입력)
    ** MAC verified OK << 뜨면 만들어졌는지 확인
6. key.pem 생성
    openssl pkcs12 -nocerts -out key.pem -in key.p12
    (p12 파일 내보낼 때 비밀번호를 지정했다면 입력)
    (해당 파일에 대한 비밀번호 지정 ( 꼭 지정해야 다음 단계 넘어갑니다))
    Enter PEM pass phrase : 비밀번호입력
  Verifying - Enter PEM pass phrase: 비밀번호 한번 더 입력
7. key.unencrypted.pem 생성
    openssl rsa -in key.pem -out key.unencrypted.pem
    (key.pem 으로 생성하는 것이므로 key.pem 에서 설정한 비밀번호 입력)
8. cat cert.pem key.unencrypted.pem > apns.pem
    (apns.pem 는 원하는 이름으로 변경 가능)

9. 6개 파일 생성 확인
10.apns.pem 파일 이용하여 Push Service 확인 - 바이두에 올리기




##### 참고 사이트 
http://qnibus.com/blog/how-to-make-certification-for-apns/