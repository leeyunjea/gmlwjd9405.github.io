---
layout: post
title: '[GitHub] GitHub로 협업하는 방법[2]'
subtitle: 'GitHub에서 Forking Workflow를 이용하여 협업하는 방법'
date: 2017-10-28
author: heejeong Kwon
cover: '/images/github-collaboration1/github-collaboration-main.png'
tags: github 협업방법
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - Forking Workflow의 개념을 파악한다.
> - Forking Workflow로 협업하는 방법을 익힌다.


# Forking Workflow
* 모든 프로젝트 참여자가 개인적인 로컬 저장소와 공개된 자신의 원격 저장소(하나의 중앙 저장소를 각자가 Fork한 것), 즉 두 개씩의 Git 저장소를 가지는 방식이다.
* 모든 코드 기여자가 하나의 중앙 저장소에 푸시하는 것이 아니라, 각자 자신의 원격 저장소에 푸시하고, 프로젝트 관리자만 다른 개발자들의 기여분을 중앙 원격 저장소에 병합할 수 있다는 점이 가장 큰 특장점이다.
  * 소규모의 팀에서는 팀원 모두가 프로젝트 관리자가 되어 중앙 저장소를 관리할 수 있다.
* 아주 큰 규모의 분산된 팀에서도 안전하게 협업하기에 좋은 협업 방법
* 오픈 소스 프로젝트에서 많이 사용하는 방식


## 1. 중앙 원격 저장소, 자신의 원격 저장소, 로컬 저장소의 개념
* 중앙 원격(remote) 저장소: 여러 명이 같은 프로젝트를 관리하는 데 사용하는 그룹 계정의 중립된 원격 저장소
  * Organization을 만드는 방법은 GitHub 페이지 오른쪽 위에 있는 "+" 아이콘을 클릭하고 메뉴에서 "New organization"을 선택하면 된다.
  * Organizatoin의 사용자와 저장소는 팀으로 관리되고 저장소의 권한 설정도 팀으로 관리한다.
* 자신의 원격(remote) 저장소: 보통 remote repository라고 불린다. 파일이 GitHub 전용 서버에서 관리되는 원격 저장소
* 로컬(local) 저장소: 보통 local repository라고 불린다. 내 PC에 파일이 저장되는 개인 전용 저장소
![](/images/github-collaboration2/github-collaboration-1.png)


## 2. 중앙 원격 저장소를 포크(fork)해서 자신만의 원격 저장소를 만든다.
중앙 원격 저장소를 복제한 저장소는 개인의 공개 저장소(remote repository) 역할을 한다.  
다른 개발자는 이 원격 저장소에 푸시할 수 없다(내려 받는 것은 가능하다).
![](/images/github-collaboration2/github-collaboration-2.png)


## 3. 프로젝트 참여자는 git clone 명령으로 로컬 저장소를 만든다.
git clone 명령으로 자신의 원격 저장소(remote repository)를 복제하여 로컬 저장소(local repository)를 만들 수 있다. 프로젝트 참여자는 이 로컬 저장소에서 작업을 수행한다.
~~~javascript
$ git clone [내 remote repository URL]
~~~
![](/images/github-collaboration2/github-collaboration-3.png)


## 4. 두 개의 원격 저장소를 연결한다.
하나는 포크한 자신의 원격 저장소이고, 다른 하나는 프로젝트 중앙 원격 저장소이다.  
이름은 아무렇게나 붙여도 되지만, 일반적으로 포크한 원격 저장소(remote repository)는 origin(git clone할 때 자동으로 만들어진다), 프로젝트 중앙 원격 저장소는 upsteram으로 붙이는 것이 일반적이다. upstream 별칭은 자동으로 생성되지 않으므로, 아래의 명령을 참고해서 직접 지정해줘야 한다.  
이렇게 연결해 줘야만 로컬 저장소(local repository)를 프로젝트 중앙 원격 저장소와 같은 상태로 유지할 수 있다.
~~~javascript
$ git remote add upstream [중앙 원격 저장소 URL]
~~~
![](/images/github-collaboration2/github-collaboration-4.png)


## 5. 설명을 위해 현재 로컬에서 작업 중인 branch 위치를 표시한다.
여기서는 중앙 원격 저장소에 develop이라는 branch를 만들었다. develop branch에서 큰 단위의 기능이 구현이 되면 master에 병합한다.  
![](/images/github-collaboration2/github-collaboration-5.png)


---
<mark>TIP</mark>  
<span style="color:#4d0000">*중앙 원격 저장소에서 develop branch를 만드는 이유*</span>  
=>

---


## 6. 새로운 기능 개발을 위해 격리된 branch를 만든다.
로컬 저장소에서 branch를 따고, 코드를 수정하고, 변경 내용을 커밋한다.
~~~javascript
$ git checkout -b [branch name]

# 위의 명령어는 아래의 두 명령어를 합한 것
$ git branch [branch name]
$ git checkout [branch name]
~~~
![](/images/github-collaboration2/github-collaboration-6.png)


## 7. 로컬 저장소의 커밋 이력을 자신의 원격 저장소(remote repository)에 푸시한다.
기능을 구현한 후 커밋한 이력을 푸시할 때는 프로젝트의 중앙 원격 저장소가 아니라, 자신의 원격 복제본 저장소(remote repository)에 푸시한다.  
~~~javascript
$ git commit -a -m "Write commit message"

# 위의 명령어는 아래의 두 명령어를 합한 것
$ git add . # 변경된 모든 파일을 스테이징 영역에 추가
$ git add [some-file] # 스테이징 영역에 some-file 추가
$ git commit -m "Write commit message" # local 작업폴더에 history 하나를 쌓는 것
~~~
자신의 원격 저장소에 변경 내용을 올리기 전까지는 변경 내용은 누구에게도 공개되지 않는다. 자신의 원격 저장소에 변경 내역을 올려서 다른 개발자가 볼 수 있도록 한다. origin으로 자신의 원격 저장소를 이미 등록해두었으므로 다음 명령만 하면 된다.
~~~javascript
$ git push origin [branch name] # branch name에 해당하는 branch를 자신의 원격 저장소에 푸시
~~~
![](/images/github-collaboration2/github-collaboration-7.png)


## 8. 프로젝트 관리자(소규모 팀에서는 모두가 관리자가 될 수 있음)에게 자신의 기여분을 반영해 달라는 풀 리퀘스트를 던진다.
프로젝트 관리자에게 자신의 기여분을 중앙 원격 코드 베이스에 반영해 달라고 요청해야 한다. GitHub 페이지에서 "Pull requests" 버튼을 이용하면, 어떤 branch를 제출할 지 정할 수 있다. 보통 이번에 추가한 기능 branch를 프로젝트 중앙 원격 저장소의 master branch에 병합해 달라고 요청할 것이다. (여기서는 프로젝트 중앙 원격 저장소의 develop branch에 pull request를 날린다.)  
![](/images/github-collaboration2/github-collaboration-8.png)


---
<mark>TIP</mark>  
<span style="color:#4d0000">*프로젝트 관리자가 소수인 경우*</span>  
=> 수정~  
=> 풀 리퀘스트는 기여한 코드에 대한 의견을 주고 받는 좋은 채널이 된다.  
=> 첫째, 공식 저장소에 기여받은 코드를 병합할 때는 프로젝트 관리자는 기여자의 변경분을 자신의 로컬 저장소로 내려 받고, 기존 코드 베이스에 부작용을 일으키지 않는 지 확인한 후, 로컬 master branch에 병합하고, 프로젝트 공식 저장소의 master branch에 반영한다.  
이제 기여분이 반영된 공식 코드 베이스를 다른 기여자들도 내려 받아 작업을 계속할 수 있다.  
=> 둘째, 프로젝트 관리자에게 자신의 기여분을 공식 코드 베이스에 반영해 달라고 요청해야 한다. Bitbucket의 ‘풀 리퀘스트’ 버튼을 이용하면, 어떤 branch를 제출할 지 정할 수 있다. 보통 이번에 추가한 기능 branch를 프로젝트 공식 저장소의 master branch에 병합해 달라고 요청할 것이다.

---

## 9. 중앙 원격 저장소와 자신의 로컬 저장소를 동기화하기 위해 로컬 저장소의 branch를 master branch로 이동한다.
![](/images/github-collaboration2/github-collaboration-9.png)


## 10. 중앙 원격 저장소의 코드 베이스에 새로운 커밋이 있다면 다음과 같이 가져온다.
메인 코드 베이스가 변경되었으므로, 프로젝트 참여하는 모든 개발자가 자신의 로컬 저장소를 동기화해서 최신 상태로 만들어야 한다.
~~~javascript
$ git pull upstream develop

# 중앙 원격 저장소에 develop branch를 만들지 않은 경우
$ git pull upstream master
~~~
![](/images/github-collaboration2/github-collaboration-10.png)

## 11. 새로운 기능을 추가하기 위해서 그 작업에 대한 branch를 생성하여 작업한다.
중앙 원격 저장소와 동기화된 로컬 저장소의 master branch에서 새로운 작업에 대한 branch를 생성하여 작업을 시작한다.
![](/images/github-collaboration2/github-collaboration-11.png)


# References
> - [http://blog.appkr.kr/learn-n-think/comparing-workflows/](http://blog.appkr.kr/learn-n-think/comparing-workflows/)
> - [https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)
> - [https://github.com/Taeung/git-training](https://github.com/Taeung/git-training)