---
title: Git repository 정리하기
date: 2022-07-06 13:06:00 +0900
categories: [Git]
tags: [GitHub] 
author: author_id 
---

## repository 병합하기
---
>git을 본격적으로 사용하다보니 처음 git을 시작했을때 생각없이 올린 repository를 정리해 카테고리별로 합쳐야 겠다는 생각이 들었다. repository들을 모두 clone하여 하나의 폴더에 합치고 다시 push하기엔 일이 너무 많을 것 같아 git bash로 병합하는 법을 알아보자!

![Desktop View](/assets/img/2022.07/06-1.jpg){: width="70%" } 
---
OOP_2020 repository에 OOP_Training_03 repository를 폴더로 넣어줄 것 이다.

## Step 1. 남겨둘 repository로 이동한다.
---
```bash
$cd [남겨둘 repository경로]
```
```bash
$cd ~/Desktop/pre_project/OOP_2020
```
OOP_2020 repository에 합칠 것이기 때문에 위의 경로로 설정하였다.

## Step 2. repository를 병합한다.
<br>
```bash
$git remote add [합칠 repository 이름] [합칠 repository 주소]
$git fetch [합칠 repository 이름]
$git merge --allow-unrelated-histories [합칠 repository 이름/합칠 branch 이름]
```
git bash에 이와같이 작성하면 된다.  
(git bash에 복사한 주소를 붙여넣을 때는 ctrl + shift +  insert 사용한다.)
<br>
```bash
$git remote add OOP_Training_03 https://github.com/ssstopeun/OOP_Training_03.git
$git fetch OOP_Training_03
$git merge --allow-unrelated-histories OOP_Training_03/master
```
내 경우 이렇게 해주었다.

합쳐진 repository는 git에서 setting으로 직접 삭제했다. git bash로 삭제하는 법도 있겠지만... 그냥 지울래.
## 마무리
---
처음에 아무것도 모르고 push 했던 것들 때문에 정리하는 시간을 가져보았다.  이제 한 카테고리 정리해서 나머지도 얼른 정리해야겠다.  
하고보니 별게 아닌데 처음엔 /master인데 /main으로 하는 등 오류가 장난아니었다. 다음에 병합할땐 보다쉽게 할 수 있겠지?ㅠㅠ. git에 익숙해지자!
