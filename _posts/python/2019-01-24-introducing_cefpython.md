---
layout: post
title: cefpython 소개(작성중)
tags: [python]
comments: true
---
## 배경
작년부터 회사에서 맡아서 하고 있는 서브 프로젝트가 있는데, 이 프로젝트에서 데스크탑 어플리케이션을 만들고 있습니다. 일을 시작하고 난 후부터 파이썬을 주로 사용해 왔기 때문에, 이 프로젝트 역시 파이썬을 이용해서 진행하고 있습니다. 보통 PyQt를 많이 사용하시는 것 같은데, 상용 제품 개발 시 PyQt의 라이센스 문제 때문에 python2.7 + PySide로 개발을 해왔습니다. 그러던 중 python3로 넘어가야 하는 이유가 생겨서, 다른 gui 오픈소스를 찾아보던 중 cefpython에 입문하게 되었습니다. 사용하면서 유용하다는 생각이 들어서, 오늘 소개하고자 합니다.

## CEF
아마 대부분의 분들이 CEF라는 것이 익숙하지는 않을 것이라고 생각됩니다. 그래서 먼저 CEF에 대해 간단하게 소개하려고 합니다. <br>
* CEF는 Chromium Embedded Framework의 약자입니다.
* 약자를 보면 알 수 있듯이, Chromium 브라우저를 다른 어플리케이션에 임베딩시켜 줍니다.
* 즉, html/css/js를 이용하여 GUI를 표현하는 크로스 플랫폼 데스크탑 어플리케이션을 만들 수 있습니다.
* 백엔드 로직은 다른 언어로 구현할 수 있습니다.
* 대표적으로 nodejs 바인딩인 Electron이 있습니다. 이 외의 다양한 언어의 바인딩이 오픈 소스로 존재합니다. (delphi, golang, java, .net, python, ruby, swift 등)

## CEFpython?
위에서 설명한 CEF의 python 바인딩입니다. GUI는 html/css/js를 이용하여 구현하고, 백엔드 로직은 python을 이용하여 구현합니다. [github](https://github.com/cztomczak/cefpython)에서 문서와 코드를 볼 수 있고, Czarek Tomczak 이라는 폴란드 개발자가 2012년부터 개발해 오고 있습니다. 사용자가 많지는 않아서 star가 1382개 밖에 안되지만, 메인 개발자가 약 7년간 꾸준히 개발을 해오고 있고 google groups에 질문을 올리면 보통 2~3일 안에 메인 개발자에게 답변이 오는 것으로 봐서는 사용해 볼만 하다고 판단했습니다. 물론 앞으로 계속 확인해봐야겠지만요.

### 구조
* 멀티 프로세스 구조로 되어 있습니다.
    * Main process: "Browser" 프로세스라고 부르며, 이 프로세스에서 파이썬이 실행됩니다.
    * Sub Process: "subprocess" 라고 부르며 renderers, plugins, GPU 등이 실행됩니다.




### Reference
* [Wikipedia](https://en.wikipedia.org/wiki/Chromium_Embedded_Framework)
* [github](https://github.com/cztomczak/cefpython)