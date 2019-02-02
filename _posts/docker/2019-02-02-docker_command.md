---
layout: post
title: 헷갈리고 자주 까먹는 docker command
tags: [docker]
comments: true
---
헷갈리고 자주 까먹는 도커 커맨드를 정리하는 글

~~~bash
// none인 이미지들 지우기
sudo docker rmi $(sudo docker images -f “dangling=true” -q)
~~~
