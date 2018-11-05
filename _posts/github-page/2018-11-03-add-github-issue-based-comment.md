---
layout: post
title: 깃헙 페이지(github page)에 깃헙 이슈 기반 댓글 기능 추가
tags: [github page]
comments: true
---

보통 깃헙 페이지 블로그에 댓글 기능을 추가한다고 하면 **Disqus**를 많이 사용하는 것을 볼 수 있다.
뭔가 남들과 다른 것을 써보고 싶었기 때문에, 다른 방법은 없나 찾아 보던 중, github issue를 기반으로 댓글 기능을 추가할 수 있는 프로젝트를 알게 되었다!
현재 이 블로그에서 제공하는 댓글 기능 또한 이 프로젝트를 이용한 것이다.

### [Utterence](https://utteranc.es/)
![](https://user-images.githubusercontent.com/30650374/47953144-a8954280-dfbc-11e8-9909-5defaa6fec85.png)
* 오픈 소스 프로젝트 - [깃헙 링크](https://github.com/utterance/utterances)
* 간단한 작동 방식 설명
    * github의 issue search API를 이용해서 등록된 깃헙 username, repository의 이슈로 찾아가 댓글을 등록한다.
    * 이슈가 없으면, [utterance bot](https://github.com/utterances-bot)이 생성해주니 걱정하지 말자.
* 사용법
    * Configuration 파트를 천천히 따라하면, 마지막에 아래와 같은 코드가 생기는데, copy를 누르고 가져다가 사용하면된다.
    ![](https://user-images.githubusercontent.com/30650374/47996903-69422f80-e13d-11e8-9ae6-2045ffe426aa.png)
    * 나의 경우 `_include`폴더에 `comments.html` 파일을 만들고 붙여넣기한 후, `_layouts` 폴더의 `_post.html` 파일의 마지막에 템플릿을 추가하였다. 
    ![](https://user-images.githubusercontent.com/30650374/47997040-dce43c80-e13d-11e8-9bb0-f1445cd5388f.png)