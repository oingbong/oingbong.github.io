---
layout: post
title: github blog with jekyll
---

### 블로그 기본 설정 및 깃허브와 연동하기

**1.지킬로 설치하기**
<br>`[sudo] gem install jekyll`

*RubyGems 버전 에러나는 경우*
<br>`[sudo] gem update --system`

*루비 버전 에러 발생하는 경우*
<br>`curl -L https://get.rvm.io | bash -s stable` : 명령어 이후 터미널 종료 및 다시 실행
<br>`rvm` : 명령어로 제대로 설치되어있는지 확인 (not found ~ 뜨면 설치 안된 것입니다.)
<br>`rvm install ruby` : 시간이 꽤 오래 걸립니다.
<br>`ruby -v` : 명령어로 버전 확인합니다.

**2.깃허브 원격저장소 만들기**
<br>GitHub.com 접속
<br>New repository 클릭
<br>Repository name : 깃허브아이디.github.io
<br>Create repository 클릭

**3.지킬로 실행하기**
<br>`jekyll new 깃허브아이디.github.io`
<br>`cd 깃허브아이디.github.io`
<br>`jekyll serve --watch`
<br>`localhost:4000` : 주소창에 치고 확인해보기

*minima 에러 발생하는 경우*
<br>~~Error Message : Could not find gem 'minima (~> 2.0)' in any of the gem sources listed in your Gemfile.~~
<br>`gem install minima`


*jekyll-feed 에러 발생하는 경우*
<br>~~Error Message : Could not find gem 'jekyll-feed (~> 0.6)' in any of the gem sources listed in your Gemfile.~~
<br>`gem install jekyll-feed`


**4.github(원격 저장소)와 jekyll(블로그 관리) 연결하기**
<br>`git init`
<br>`git remote add origin 깃허브저장소URL`
<br>`git add . `
<br>`git commit -m "Initialize blog"`
<br>`git push origin master`

**5.블로그 주소확인해보기**
<br>깃허브아이디.github.io : 주소창에 확인해보기 (3번의 로컬에서 확인했던 화면과 동일하면 성공)
