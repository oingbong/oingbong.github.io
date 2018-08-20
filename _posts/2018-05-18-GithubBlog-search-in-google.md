---
layout: post
title: 깃허브 블로그 구글 검색 가능하게 하기
tags:
- Github
---

### 구글 웹마스터 도구(Search Console)에 속성 추가 및 인증
1. 구글 웹 마스터 도구 접속 (https://www.google.com/webmasters/tools/home?hl=ko)
2. 블로그 주소 입력 및 속성추가 버튼 클릭
3. 구글에서 제공하는 html 다운로드
4. 해당 파일을 자신의 GitHub jekyll 블로그 루트 디렉토리에 저장 및 커밋,푸시
5. Search Console 화면에서 확인버튼 클릭

### sitemap.xml 파일 생성 및 _config.xml 수정
1. _config.xml 파일의 url 부분에 블로그 url 입력 및 저장
2. sitemap.xml 에 아래 내용 입력

```
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    {% for post in site.posts %}
    <url>
        <loc>{{ site.url }}{{ post.url | remove: 'index.html' }}</loc>
    </url>
    {% endfor %}

    {% for page in site.pages %}
    {% if page.layout != nil %}
    {% if page.layout != 'feed' %}
    <url>
        <loc>{{ site.url }}{{ page.url | remove: 'index.html' }}</loc>
    </url>
    {% endif %}
    {% endif %}
    {% endfor %}
</urlset>
```

### 구글 웹마스터 도구(Search Console)에 sitemap.xml 제출
1. 구글 웹 마스터 도구 접속
2. 왼쪽 메뉴 - 크롤링 - Sitemaps 선택
3. 오른쪽 상단 - SITEMAP 추가/테스트 선택
4. 빈칸에 sitemap.xml 입력 및 테스트
5. 테스트 문제 없으면 제출