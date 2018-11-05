---
layout: post
title: 깃헙 페이지(github page)에 이미지 삽입
tags: [github page]
comments: true
---
깃헙 페이지에 이미지를 삽입하고 싶을 때, 레포지토리에 이미지를 넣고 해당 링크를 참조한다면 레포지토리의 용량이 점점 늘어날 것이다. 물론 나처럼 포스팅이 별로 없다면 괜찮겠지만...<br>
그렇지만 레포지토리 용량이 커지는 것이 싫기 때문에... 레포지토리에 이미지를 복사하지 않고 글에 이미지를 삽입하는 방법을 알아 보았다.

1. 일단 이미지를 준비하고 copy한다..(crtl + c)
2. 그리고 깃헙 레포지토리의 이슈 페이지로 가서 커멘트란에 붙여넣기 한다..(ctrl + v)
3. 그러면 링크가 뜰텐데, 그 링크를 마크다운에서 사용하시면 레포지토리에 이미지를 복사하지 않고 링크를 참조하는 것만으로 이미지를 삽입할 수 있게 된다.
![asd](https://user-images.githubusercontent.com/30650374/47948585-a0b2af80-df76-11e8-8ce3-fb5b89b49831.png)

저 링크만 따서 사용하고, "Update comment"는 하지 않아도 된다. 붙여넣기 하면 이미지가 바로 깃헙 서버에 업로드 되고 해당 리소스의 url을 주는 방식이여서 그런 것 같다. 

### Reference
<https://songgane.github.io/tips/2015/07/12/host-image-to-github/>

