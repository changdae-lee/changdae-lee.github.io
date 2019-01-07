---
layout: post
title: 스레드 안전(Thread-safety)이란?
tags: [operating system]
comments: true
---
최근 python의 GIL(Global Interpreter Lock)을 정확히 설명하지 못해서 공부하던 중, 들어는 봤으나 역시 정확한 내용은 몰랐던 스레드 안전(Thread-safety)에 대해 글을 써보려고 한다. 운영체제 수업 때 들어본 기억이 있는 것 같은데, 수업을 날로 들었는지 기억이 나지 않는다...

#### 위키 정의
> *스레드 안전은 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다. 보다 엄밀하게는 하나의 함수가 한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 스레드에서의 함수의 수행 결과가 올바로 나오는 것으로 정의한다.*

위키 정의만 봐서는 대충 어떤 뜻인지 파악하는데도 몇 번을 읽어본 것 같다. 간단한 코드를 통해 알아보겠다.
```css
a = 3
b = 2
print(a + b)
```
**여러 스레드가 위의 코드를 동시에 실행하더라도 각 스레드에서 출력되는 값이 5로 동일하다면 thread-safety 하다고 할 수 있다고 이해했다.**

위와 같이 간단한 연산의 경우, 여러 스레드가 동시에 처리하더라도 thread-safety 하게 처리될 수 있다. (뒷단에서 실제로 thread-safety한지는 정확히 몰라도 말이다...) 하지만 코드가 조금 복잡해지면 어떨까?

```css
d[key] += value
d[key] = d[key] + value
d[key] = d[key2]
```
**위의 코드는 thread-safety하지 않다.** 딕셔너리의 조회와 대입 사이에 스레드 전환이 일어날 수 있기 때문이다.

그렇다면 위와 같은 경우에서 thread-safety를 구현할 수 있는 방법에는 어떤 것들이 있을까? 위키를 보면 다음과 같은 기법들을 통해 thread-safety를 보장할 수 있다고 한다.
* **Re-entrancy**
    * 어떤 함수가 한 스레드에 의해 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하더라도 그 결과가 각각에게 올바로 주어져야 한다.
    * 설명하기가 좀 어려운데, 결국 스레드끼리 독립적일 수 있게 코드를 조심히 잘 짜라는 것 같다...
* **Thread-local storage**
    * 공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소들을 사용함으로써 동시 접근을 막는다.
    * 멀티 스레드 환경에서 글로벌 변수 등의 사용을 지양하라는 것으로 이해하면 될 듯하다.
* **Mutual exclusion**
    * 공유 자원을 꼭 사용해야 할 경우 해당 자원의 접근을 세마포어 등의 락으로 통제한다.
    * 동기화 객체. python의 threading.Lock, java의 synchronized 등
* **Atomic operations**
    * 공유 자원에 접근할 때 원자 연산을 이용하거나 '원자적'으로 정의된 접근 방법을 사용함으로써 상호 배제를 구현할 수 있다.
    * 쉽게 설명하자면 "+=" 연산의 경우, 한 줄의 코드더라도 '+' 연산 후에 '=' 연산을 하기 때문에 원자적이라고 하기 어렵다. (실제 virtual instruction 수준에서 보더라도 원자적이라고 보기 어렵다.) 


**thread-safety가 자명한 경우를 제외하고는 동기화 객체(Mutual exclusion)를 사용하는 것을 추천한다.** 아무래도 느려질 가능성이 생기긴 하겠지만, 실 상황에서 나머지 3가지를 자명한 수준으로 고려하거나 적용하는 것이 쉽지 않기 때문이다. 그렇다면 실 상황에서 mutual exclusion을 사용하는 예는 무엇이 있을까? 바로 **GIL**다. 

wiki.python에서는 **GIL 필요한 주요 이유가 CPython의 메모리 관리가 thread-safe하지 않기 때문**이라고 설명하고 있다.(This lock is necessary mainly because CPython's memory management is not thread-safe.)

thread-safety로 시작해서 GIL로 끝나버려서 글이 이상해진 느낌인데, 정리하자면 thread-safety란 여러 스레드가 동일한 작업을 수행할 때 서로 오염시키지 않고 각각이 원하는 결과를 출력해내는 것을 뜻하며 이를 보장하기 위해서는 thread-safety가 자명한 경우를 제외하고는 동기화 객체를 쓰는 것이 좋다.


<br>
<br>
**공부를 위해 작성한 글입니다. 틀린 내용이 있다면 적극적으로 말씀해주시면 정말 감사하겠습니다.**

### Reference
* <https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_%EC%95%88%EC%A0%84>
* <http://www.flowdas.com/blog/thread-safety-in-python/>
* <https://wiki.python.org/moin/GlobalInterpreterLock>
