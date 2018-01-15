---
layout: post
title: Git 사용방법
tags:
- Git
---


# Git 사용방법

### 정보
 - 기본 키워드 : clone / add / commit / push / pull
 - 응용 키워드 : branch / checkout / merge
 - 기본 순서 부분만 보고 git 작업이 가능하며 응용 순서 부분은 생략 가능합니다.

### 기본 순서
1. Git & 소스 관리 폴더로 위치하기
	`cd TestFolder`
2. 원격저장소에 있는 소스 로컬저장소로 가져오기
	`git clone https://github.com/git/git.git`
3. 로컬에서 소스 수정 작업 진행
4. 소스 버전관리에 예비 추가
    1) git add <수정 or 추가한 폴더나 파일명> : 현재 디렉토리 기준 특정 파일 기준
    2) git add . : 현재 디렉토리 기준 수정 or 추가한 폴더나 전체 파일 기준
5. 소스 버전관리에 추가
	1) git commit -m "<커밋 메세지 입력>" : commit 과 message 입력 동시 작업
	2-1) git commit
	2-2) i 버튼 클릭 & 콘솔 아래 insert 로 상태값 변경 확인
	2-3) 커밋 메세지 입력
	2-4) ESC 버튼 클릭 & :wq 입력 후 Enter 버튼 클릭

	**add 와 commit 동시 진행방법(4,5번)**
	: 수정하는 경우에만 해당 , 추가한 폴더나 파일은 4번부터 진행해줘야 합니다.
	`git commit -am "<커밋 메세지 입력>"`
6. 원격저장소에 추가하기 전 원격저장소에 있는 소스 가져오기 (충돌 방지)
	`git pull`

    **tracking 에러나는 경우**
    	There is no tracking information for the current branch.
		Please specify which branch you want to merge with.

        git branch --set-upstream-to=origin/branch Name (기본 master 지정)

7. 원격저장소에 추가하기
	`git remote -v` : 원격저장소 정보 확인 (git clone 을 한 경우 자동으로 정보 저장되어 있습니다.)
	`git push origin master` : Master Branch 의 내용을 origin 으로 설정된 원격저장소에 push

    **이후에는 git push 로 생략 가능합니다.**
    **사용자정보가 없는경우**
	`git config --global user.name <NickName>`
	`git config --global user.email <Email>`


### 응용 순서 1 (Branch / 로컬기준) : Branch 만들기 및 정보 확인
: 테스트, 일시적 개발, 실제 소스 반영 미확정 경우 등

1. 현재 브랜치 확인 (master 기본)
	`git branch`
2. 브랜치 생성 : 브랜치를 생성할 시점의 git commit 상태를 복사합니다.
	`git branch <Branch Name>`
3. 브랜치 이동
	`git checkout <Branch Name>`

    **브랜치 생성 및 이동 동시 진행 방법(2,3번)**
	`git checkout -b <Branch Name>`
4. add / commit 작업 진행

5. Branch 정보확인
 1) 현재 Branch 가 아닌 전체 Branch 상태를 확인
	`git log --branches --decorate`
 : 커밋로그의 커밋아이디 옆에 master 로 적혀있는 것이 master Branch 의 최신 커밋 상태
 : 커밋로그의 커밋아이디 옆에 <Branch Name> 으로 적혀있는 것이 해당 Branch의 최신 커밋 상태

 2) master Branch 와 해당 Branch 의 커밋 상태가 다를 때 상태 확인
	`git log --branches --decorate --graph`
 : 그래프를 확인해서 공통커밋상태가 어딘지 확인 가능
	** 첨부파일 git_branch1 참고 & 설명**
	1. master 브랜치의 최신커밋 5 / 이전커밋 2
	2. exp 브랜치의 최신커밋 4 / 이전커밋 3
	3. master 와 exp 의 공통커밋 2
        간단한 그래프 상태 확인
        `git log --branches --decorate --graph --oneline`

        ** 첨부파일 git_branch2 참고
    	브랜치별 상태 확인
        `git log master..<Branch Name>`
        `git log master..exp` : master 브랜치에는 없고 exp 브랜치에는 있는 커밋확인
        `git log exp..master` : exp 브랜치에는 없고 master 브랜치에는 있는 커밋확인

### 응용순서 2 (Branch / 로컬기준) : Branch 병합
 : 다른 브랜치에 있는 소스작업을 master 브랜치로 합쳐서 소스 관리 할 경우

######1단계 : master Branch 로 이동 후에 merge 를 사용합니다.######
1. master 브랜치로 이동
	`git checkout master` : master Branch 로 이동합니다.
2. exp Branch 내용을 master Branch 에 적용
	`git merge exp`
3. 내용 확인 후에 해당 내용을 입력 후 Enter 버튼을 누릅니다.
	`:wq`
4. git log
	** 첨부파일 git_merge1 참고

######2단계 : exp Branch 를 master Branch 상태와 동일하게 합니다.######
1. exp 브랜치로 이동
	`git checkout exp`
2. master Branch 내용을 exp Branch 에 적용
	`git merge master`
3. 내용 확인 후에 해당 내용을 입력 후 Enter 버튼을 누릅니다.
	`:wq`
4. git log
	** 첨부파일 git_merge2 참고

######3단계 : exp Branch 삭제하기 (생략 가능)######
1. master Branch 로 이동
	`git checkout master`
2. exp Branch 삭제
	`git branch -d exp`
3. 삭제 확인
	`git log --branches --graph --decorate --oneline`
	** 첨부파일 git_merge3 참고


### 응용순서 3 (Branch / 로컬기준) : Branch 충돌 해결
 : Branch 끼리 Merge 하는 경우 같은 부분의 내용을 수정한 경우 에러가 날 수 있습니다.
 : 아래 두가지 테스트의 내용은 exp Branch > master Branch 로 merge 하는 경우

######테스트 1 : 충돌 X######
	exp Branch 내용
		function b(){}
		function a(){}
	master Branch 내용
		function a(){}
		function c(){}

    결과 1 : 충돌 없이 Auto-merge 합니다.
    	function b(){}
    	function a(){}
    	function c(){}
######테스트 2 : 충돌 O######
	exp Branch 내용
		function b(){}
		function a(exp){}
		function c(){}
	master Branch 내용
		function b(){}
		function a(master){}
        function c(){}

    결과 2 : 충돌납니다. CONFLICT 메세지 및 Automatic merge failed 메세지


######충돌시 해결방법######
1. master Branch 로 이동합니다.
	`git checkout master`
2. vim OR 소스편집기에서 해당 파일 열기
	`vim <File Name>`
    ** 첨부파일 git_mergeCrash1 참고**
    첨부파일 설명
    `=======` 기준
	`<<<<<<<` HEAD 부분 : 현재 checkout 한 브랜치 부분
    `>>>>>>>` exp 부분 : merge 한 브랜치 부분
3. 위에 첨부파일 설명 참고하여 소스부분 직접 수정합니다.
    ** 첨부파일 git_mergeCrash2 참고**
4. vim 에서는 ESC 버튼 > :wq > Enter 버튼
5. git add <File Name>
6. git commit > :wq > Enter 버튼
** 첨부파일 git_mergeCrash3 참고 : Conflicts 난 것을 수정했다는 메세지가 보입니다.**
7. 커밋확인
	`git log`

### 기타 명령어
`git status` : 현재 버전관리 정보 확인
`git log` : 버전관리 이전 내역 확인
`cat <파일이름>` : 파일 내용 확인

커밋아이디 확인 방법
`git log` : commit 옆에 나오는 영어+숫자 조합

소스비교
`git log -p` : 각 커밋끼리의 로그 차이점을 확인
`git diff <Commit ID 1>..<Commit ID 2>` : 두개의 커밋 차이점을 확인

`git commit --amend` : 로컬저장소에 커밋한 이후 커밋메세지 변경할 때 (원격저장소에 push 하기 전)

### 참고
[생활코딩 : 지옥에서 온 Git](https://opentutorials.org/course/2708 "생활코딩")
