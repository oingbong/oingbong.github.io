---
layout: post
title: RefreshScope 적용안되는 경우
tags:
- SpringCloud
- RefreshScope
---

# RefreshScope 적용안되는 경우

#### Spring Cloud 튜토리얼에서 @RefreshScope 를 사용하여 서버를 재시작 할 필요없이 설정값을 변경하기 위해 Postman을 이용해 Refresh 를 해주는 작업이 필요한데 기존과 다른 부분이 있습니다.

## 기존방법
1.@RefreshScope 추가
2.config-server 의 config-client.properties 에서 값 변경
3.config-server 의 config-client.properties git commit
4.postman 으로 Send
Post : localhost:8982/refresh
5.확인
Localhost:8982/rest/message

## 변경방법
1.@RefreshScope 추가
2.config-client 의 bootstrap.properties 에 값 추가
management.endpoints.web.exposure.include=*
3.config-server 의 config-client.properties 에서 값 변경
4.config-server 의 config-client.properties git commit
5.postman 으로 Send
Post : localhost:8982/actuator/refresh


## 주의
__1.git commit 필요__
Config-server 의 config-client.properties 값 변경 후 commit 을 하지 않으면 값 적용이 안되며
그 이전에 postman 으로 Send 할 때 리턴값이 [] << 이렇게 나옵니다.

제대로 된 리턴값은 아래와 같습니다.
[
“config.client.version”,
“message"
]

__2.actuator 영향__
Config-client 의 bootstrap.properties 에 endpoints 와 관련한 값 추가를 해야하며
Postman 으로 Refresh 수행하는 url 도 */actuator/* 가 추가되었습니다.

## 참고
Spring Cloud 튜토리얼 : https://www.youtube.com/watch?v=b2ih5RCuxTM
Actuator & endpoint 관련 : http://wonwoo.ml/index.php/post/1947
