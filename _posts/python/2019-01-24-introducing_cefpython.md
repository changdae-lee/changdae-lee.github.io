---
layout: post
title: "Cefpython: electron for python"
tags: [python]
comments: true
---
작년부터 회사에서 맡아서 하고 있는 서브 프로젝트가 있는데, 이 프로젝트에서 데스크탑 어플리케이션을 만들고 있습니다. 일을 시작하고 난 후부터 파이썬을 주로 사용해 왔기 때문에, 이 프로젝트 역시 파이썬을 이용해서 진행하고 있습니다. 보통 PyQt를 많이 사용하시는 것 같은데, 상용 제품 개발 시 PyQt의 라이센스 문제 때문에 python2.7 + PySide로 개발을 해왔습니다. 그러던 중 python3로 넘어가야 하는 이유가 생겨서, 다른 gui 오픈소스를 찾아보던 중 cefpython에 입문하게 되었습니다. 사용하면서 유용하다는 생각이 들어서, 오늘 소개하고자 합니다.

## CEF
아마 대부분의 분들이 CEF라는 것이 익숙하지는 않을 것이라고 생각됩니다. 그래서 먼저 CEF에 대해 간단하게 소개하려고 합니다. <br>
* CEF는 Chromium Embedded Framework의 약자입니다.
* 약자를 보면 알 수 있듯이, Chromium 브라우저를 다른 어플리케이션에 임베딩시켜 줍니다.
* 즉, html/css/js를 이용하여 GUI를 표현하는 크로스 플랫폼 데스크탑 어플리케이션을 만들 수 있습니다.
* 백엔드 로직은 다른 언어로 구현할 수 있습니다.
* 대표적으로 nodejs 바인딩인 Electron이 있습니다. 이 외의 다양한 언어의 바인딩이 오픈 소스로 존재합니다. (delphi, golang, java, .net, python, ruby, swift 등)

## CEFpython?
위에서 설명한 CEF의 python 바인딩입니다. GUI는 html/css/js를 이용하여 구현하고, 백엔드 로직은 python을 이용하여 구현합니다. 
[github](https://github.com/cztomczak/cefpython)에서 문서와 코드를 볼 수 있고, Czarek Tomczak 이라는 폴란드 개발자가 2012년부터 개발해 오고 있습니다.

### 구조
cefpython의 대략적인 구조를 설명해보겠습니다. electron과 비슷한 구조를 가지는 것 같다는 느낌을 받았습니다. 
* 멀티 프로세스 구조로 되어 있습니다.
    * Main process: "Browser" 프로세스라고 부르며 이 프로세스에서 파이썬이 실행됩니다.
    * Sub Process: "subprocess" 라고 부르며 이 프로세스에서 renderers, plugins, GPU 등이 실행됩니다.

## 예제
파일 시스템에서 폴더를 선택하면 프론트에 경로가 표시되고 확인 버튼을 누르면 그 경로가 콘솔 창에 표시되는 아주 간단한 어플리케이션을 만들어 보았습니다. 
전체 코드는 [여기](https://github.com/greatleee/cefpython-example)를 참고하시면 됩니다. 아래는 예제 프로그램의 스크린샷입니다.
![](https://user-images.githubusercontent.com/30650374/51800930-4fb30e80-227a-11e9-9efa-82475aced1c1.JPG)
![](https://user-images.githubusercontent.com/30650374/51800933-5477c280-227a-11e9-9057-98cf4bb1e1bc.JPG)
![](https://user-images.githubusercontent.com/30650374/51800935-593c7680-227a-11e9-8a67-29dc14215326.JPG)

### 디렉토리 구조
* cefpython-example/
    * web/
        - index.html
        - index.css
        - index.js
    * server.py

server.py에 cefpython 관련 코드가 작성되어 있고 web 폴더 아래에 프론트 코드를 작성하였습니다.
<br> ```python ./server.py```를 통해 실행이 가능합니다.

### server.py
```python
def main():
    # python traceback을 처리하는 
    # handler를 붙여준다고 생각하면 될 것 같습니다.
    sys.excepthook = cef.ExceptHook
    cef.Initialize(settings=app_settings)
    # createbrowsersync를 통해 브라우저 창을 실행시킵니다.
    # url에 file://를 이용하여 html 파일에 접근합니다.
    browser = cef.CreateBrowserSync(url='file:///web/index.html',
                                    settings=browser_settings,
                                    window_title='Hello World!')
    # 윈도우에서는 default로 win32 api를 이용해 창을 생성하는 
    # 방식을 채택하고 있기 때문에
    # 윈도우 사이즈를 조절하기 위해 win32 api를 이용합니다.
    # 윈도우가 아닌 환경에서 실행시켜 보신다면 이 부분을 제외시키시면 됩니다.
    if platform.system() == "Windows":
        window_handle = browser.GetOuterWindowHandle()
        insert_after_handle = 0
        SWP_NOMOVE = 0x0002
        ctypes.windll.user32.SetWindowPos(window_handle,
                                          insert_after_handle, 
                                          0, 0, 700, 200, SWP_NOMOVE)
    # python 객체, 함수 등을 javascript로 바인딩합니다.
    set_javascript_bindings(browser)
    # 아마 모든 gui framework에는 message loop라는 개념이 존재할텐데,
    # 이 예제에서는 cef를 다른 gui framework에 임베딩하지 않기 때문에
    # 직접 message loop를 콜해줍니다.
    # 만약 다른 gui framework에 임베딩한다면 MessageLoop를 사용하지 않습니다.
    cef.MessageLoop()
    cef.Shutdown()


class External():
    target_dir_path = ''
    def __init__(self, browser):
        self.browser = browser

    def ask_dir_path(self):
        # win32의 file dialog를 이용하는 방법을 찾아보기 귀찮아서....
        # 가벼운 tkinter를 이용했습니다.
        # 이런 기본적인 기능들은 추가되었으면 좋겠네요.
        from tkinter.filedialog import askdirectory
        from tkinter import Tk 
        Tk().withdraw()
        self.target_dir_path = askdirectory()
        # python으로 브라우저에서 javascript 코드 실행
        self.browser.ExecuteFunction('updatePath', self.target_dir_path)


def py_confirm(path):
    print('success: ' + path)


def set_javascript_bindings(browser):
    external = External(browser)
    bindings = cef.JavascriptBindings(
            bindToFrames=False, bindToPopups=False)
    # py_confirm 함수와 External 클래스를 
    # 자바스크립트 코드로 사용할 수 있도록 설정합니다.
    bindings.SetFunction('py_confirm', py_confirm)
    bindings.SetObject('external', external)
    browser.SetJavascriptBindings(bindings)
```

### index.js
```javascript
function askDirPath(){
    // python 객체 콜
    external.ask_dir_path();
    document.getElementById("finder").disabled = true;
}

// python에서 콜되는 함수
function updatePath(targetDirPath) {
    document.getElementById("target-path").textContent = targetDirPath;
    document.getElementById("finder").disabled = false;
}

function confirm() {
    // python 함수 콜
    py_confirm(document.getElementById("target-path").textContent)
}
```

## 맺음
cefpython 프로젝트가 시작된지 7년이라는 세월이 지난만큼 앞서 소개드린 기능들과 특징들 외에 더 많은 기능들과 특징들이 존재합니다.
게다가 공식 문서가 잘 되어 있기 때문에 더 큰 어플리케이션을 만드는데 큰 무리 없이 진행해 나가실 수 있을 것이라고 생각합니다.
<br>메인 컨트리뷰터가 한 명 뿐이라는 점, 스타가 1300개 밖에 없다는 점, 다른 gui 프레임워크를 사용해야 한다는 점 등 불편한 점도 많이 있습니다.
저도 앞으로 계속 사용해보면서 검토를 해나갈 예정이고, 이 글을 통해 사용자가 늘어났으면 하는 바람이 있기도 합니다.
가능하다면 불편한 점들은 개선해 컨트리뷰트를 해보고 싶기도 합니다. 
<br>물론 대안이 존재하지 않은 것은 아닙니다.
[eel](https://github.com/ChrisKnott/Eel), [pywebview](https://github.com/r0x0r/pywebview) 등이 더 존재합니다.
하지만 cef 또는 chromium을 이용한 오픈 소스는 cefpython 뿐이더군요.
eel은 사용중인 os의 default 브라우저를 사용하거나 chrome이 설치되어 있다면 chrome을 사용하고, pywebview는 explorer를 사용합니다. 
<br>[electron + python boilerplate](https://github.com/fyears/electron-python-example), [electron + python-shell boilerplate](https://www.techiediaries.com/python-electron-tutorial/)도 존재합니다.
만약 cefpython 사용이 꺼려지신다면 방금 소개드린 것들을 사용해보시는 것도 좋을 것 같습니다.


## Reference
* [Wikipedia](https://en.wikipedia.org/wiki/Chromium_Embedded_Framework)
* [github](https://github.com/cztomczak/cefpython)
* <https://blog.hanlee.io/2017/evolution-of-electron/>
* <https://tinydew4.github.io/electron-ko/docs/tutorial/quick-start/>