---
layout: post
title: Github Collaboration
---

# 다른 사람의 프로젝트에 내용을 추가할 때

## 1. Fork
다른 사람의 프로젝트를 복사해오는것을 뜻함.
```
보통 웹사이트에서 한다.
```

## 2. Clone
Fork해서 Repository로 가져온 프로젝트를 로컬 컴퓨터로.
```
$git clone <origin repository>
```

## 3. Upstream
Clone을 했을 경우, fork 한 내 프로젝트는 origin이 되어버린다.
진짜 original은 다음과 같이 upstream이라 칭한다(convention).
```
$git remote add upstream git://github.com/<UpstreamUser>/<UpstreamRepository.git> 

$ git remote -v

```

## 4. Branch
Branch 생성을 통해 고립된 환경 추가.

```
$git checkout -b <branch-name>

$ git branch

```

## 5. Push
add와 commit을 완료 후에, origin에게 <branch-name>으로 push를 한다.
```
$ git push origin <branch-name>
```

## 6. Pull Request
다른 사람의 프로젝트에 바꿔달라고 요청하는걸 뜻함.
```
보통 웹사이트에서 한다.
```
-----------------------

# 다른 사람의 프로젝트에 내용을 받아올 때

## 1. Upstream으로부터 'fetch'로 파일들을 받아온다.
```
$git fetch upstream
```

## 2. 받아온 파일들을 main branch로 checkout 합니다.
```
$git checkout main
```

## 3. checkout 후, upstream/main 과 합칩니다.
```
$git merge upstream/main
```

## 4. origin repository에게 push 합니다.
```
$git push
```
