---
layout: post
title: Visual Studio Live Server
date: 2021-09-04 17:10:10
---

웹 프로그래밍 중 쓸만한 extension 중에서는 HTML의 태그를 빠르게 만들어 낼 수 있는 Emmet extension이라던가, Node.js에서 제공하는 live server과 같은 것이 있다.  

Visual Studio IDE에서는 이 Live Server 자체는 없고, 그 대신, Browser link라는 것이 존재한다.  

Browser Link는 Tools > Options > Web을 살펴보다보면 Enable 할 수 있게 되는데, VS의 debug option(메뉴 아이템 중 새로고침같이 생긴모양)에서 Enable 되었는지 확인 할 수 있다.  

Link가 잘 진행 되었을 시는, CSS 스타일을 바꿀 시는 auto sync가 적용되는 모양이지만, html, aspx, cshtml과 같은 페이지들을 저장할 때는 reload on save가 적용되지 않는다.  

이 때, 저장 후 CTRL+ALT+Enter를 눌러 manual refresh를 IDE에서 브라우저로 signalR을 이용해 시그널을 보내는 방법도 있겠지만, 이 또한 불편하기에, 세이브 시 리로드도 같이 수행하는 extension이 존재한다.  

이 Extension의 이름은 [Browser Reload On Save](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BrowserReloadonSave)이다.

다운로드를 끝마치면, Tools > Options > Web의 Browser Reload On Save를 Enable 해주면 된다.