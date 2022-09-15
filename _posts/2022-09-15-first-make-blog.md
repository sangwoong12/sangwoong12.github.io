---
layout: post
title: Git 블로그 만들기 - jekyll
date: 2022-09-15 19:00:00 +0800
last_modified_at: 2022-09-15 19:30:00 +0800
tags: [git]
toc:  true
---

# github를 활용해서 블로그 만들기

## 1. github repository 생성

github 아이디 생성이후 로그인이후 <a href="github.com/new">[repository 생성]</a>를 누른다.

<img src="/images/first-make-blog/1.png" width="80%" height="80%">

> 1. 빨간 글씨로 적혀있는 부분에 자신의 Owner이름.github.io 라고 작성한다.
> 2. Public 로 체크 하고 Add a README file 를 체크한다.

이후 생성하면된다. 잘 동작하는지 확인하기 위해 "자신의 이름.github.io"로 접근해서 화면이 뜬다면 성공한 것이다.

## 2. 자신에게 맞는 jekyll 테마 찾기

<a href="http://jekyll-themes.com/free/">http://jekyll-themes.com/free/</a>에서 찾아보는 것을 추천한다. 여러가지가 존재하고 demo를 통해 동작방식을 확인할 수 있다.

필자는 <a href="https://jekyll-themes.com/not-pure-poole/">https://jekyll-themes.com/not-pure-poole/</a>를 사용했다.

<img src="./images/first-make-blog/1.png" width="80%" height="80%">

> 1. 사진에 보이는 DOWNLOAD 를 눌려 다운로드 한다.

## 3. GitHub Desktop 활용하기 (Window 10)

Git Hub Desktop은 처음 접하는 Git을 쉽게 사용할 수 있도록 도와주는 앱이다.

그중 로컬 파일과 git repository를 연동하는 작업을 할것이다.

<a href="https://desktop.github.com/">https://desktop.github.com/</a>로 들어가서 Download for Windows (64 bit) 눌려 설치를 한다.

설치이후

<img src="/images/first-make-blog/3.png" width="80%" height="80%">

그림처럼 검색하여 앱을 실행한다.

<img src="/images/first-make-blog/4.png" width="80%" height="80%">

실행하면 다음과 같은 화면이 등장한다. 
> 1. 좌측 상단에 있는 current repository에 있는 ▼를 클릭한다. 
> 2. 이후 Add ▼ 를 눌려 Clone repository를 누른다.

<img src="/images/first-make-blog/5.png" width="80%" height="80%">

해당 화면이뜨게 되는데 연동할 로컬 파일을 선택하고 처음 생성했던 자신owner.github.io 를 선택한다.
필자의 경우 선택하게 되면 Local path란에 C:\00gitblog\sangwoong12.github.io 라고 나타난다. 마지막으로 clone을 누르면 된다.



## 4. 다운받은 jekyll 테마 적용하기

마지막으로 다운받은 jekyll 테마를 적용해보겠다.

<img src="/images/first-make-blog/6.png" width="80%" height="80%">

다운받은 파일을 Github Desktop에서 clone 해준 파일에 압축을 해제한다.
필자의 경우 위의 사진처럼 압축을 풀어 주었다.

git에 변동사항을 적용시키기 위해 다시 Github Desktop을 보게되면 

<img src="/images/first-make-blog/7.png" width="80%" height="80%">

그림과 같이 변동사항이 있어 changed files 란에 변동된 파일들이 있을 것이다.
필자의 경우 보여주기 위해 사진한장만 변경하여 하나밖에 없지만 위 내용대로 했을경우 여러개의 파일이 있을 것이다.

이후 좌측 하단에 필자는 "적용하기 위한 commit" 이라고 적었는데 아무거나 적어도 된다.
이후 commit to main을 누른다.

<img src="/images/first-make-blog/8.png" width="80%" height="80%">

사진처럼 뜨게 될텐데 가운데 Push origin를 누르게 되면 끝이다.
이후 자신owner.github.io로 접근하게 되면 적용이 된다. 참고로 적용하는데 시간이 5~10분 정도 소요된다.
