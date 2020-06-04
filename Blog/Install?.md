# 설치하다; Java 설치; Python 설치; Node.js 설치; C 설치?
Java 프로그래밍을 하기 위해 가장 처음 하는 일은 뭘까?

보통 Java 설치 방법을 구글에 검색한다. Python, Go, Ruby, PHP라고 다를 게 없다.

 이러한 과정에 대해 한 번도 이상하다 생각한 적이 없었는데, https://python.bakyeono.net/chapter-1-1.html 에서 다음과 같은 문구를 읽고 의문이 생겨 깊게 생각해봤다.
 > 파이썬 인터프리터 프로그램을 줄여서 ‘파이썬’이라고 부를 때가 많다. ‘파이썬’이라고 하면 언어를 뜻할 때도 있고, 인터프리터 프로그램을 뜻할 때도 있다.

**Java 설치**라는 말에 대해 자세히 알아본다.

## Java 설치
구글에 다음과 같이 [Java 설치](https://www.google.com/search?q=Java%20%EC%84%A4%EC%B9%98)라고 검색하면 많은 내용이 나온다.

Java는 프로그래밍 언어다.
> 자바는 썬 마이크로시스템즈의 제임스 고슬링(James Gosling)과 다른 연구원들이 개발한 객체 지향적 프로그래밍 언어이다.
> [https://ko.wikipedia.org/wiki/자바_(프로그래밍_언어)](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4))

**Java 설치**를 풀어서 보면, 프로그래밍 언어를 설치한다는 뜻이 된다.

하지만, 프로그래밍 언어를 설치한다는 것은 근본적으로 말이 안 된다.
프로그래밍 언어는 사람과 컴퓨터가 대화할 수 있는 **언어**를 말한다.

**프로그래밍 언어를 해석할 수 있는 해석기(컴파일러, 인터프리터)를 설치한다는 것이 더 말이 된다.**
> ![설국열차](https://user-images.githubusercontent.com/9030565/83703224-daafd000-a649-11ea-8a2e-181f054da9b7.png)
> 영화 설국열차를 보면 한국인 남궁민수는 외국인들과 대화하기 위해 **언어 번역기**를 사용한다.
> 한국어로 말해도 영어로 번역되어 나오기 때문에 외국인들이 이해할 수 있다.
> 여기서 남궁민수는 **영어**를 자신의 뇌 속에 설치하지 않는다. 또한 외국인들도 **한국어**를 그들의 뇌 속에 설치하지 않는다.
> **각 언어를 해석해서 변환할 수 있는 번역 소프트웨어가 설치된 번역기를 사용한다.**

Java의 경우 [JDK]([https://namu.wiki/w/JDK](https://namu.wiki/w/JDK))를 설치한다는 표현이 더 옳다.

Java 언어로 작성된 소스코드를 해석하고 실행할 수 있는 소프트웨어인 JDK(Java Development Kit)를 설치한다는 표현이 맞는 것이다.

---

![JDK](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQXHBIT4DdqeuqI1-NuJUaY1PdxRwWhQAy7_qfu5xYj6FNiF-1V&usqp=CAU)
> [https://www.javacodemonk.com/what-is-difference-between-jdk-jre-and-jvm-6380989d](https://www.javacodemonk.com/what-is-difference-between-jdk-jre-and-jvm-6380989d)

## Python 설치
Java와 비슷한 방법으로 Python 프로그래밍을 하기 위해서 우리는 [Python 설치](https://www.google.com/search?q=Python%20%EC%84%A4%EC%B9%98)를 검색한다.

> Python is a programming language that lets you work quicklyand integrate systems more effectively.
> https://www.python.org

Python도 Java와 비슷하다. Python 언어를 설치하는 것이 아니라 Python 언어로 작성된 프로그램을 실행시킬 수 있는 환경(소프트웨어, 해석기)을 설치하는 것이다.

Python은 보통 인터프리터 기반이라고 알고 있지만, 인터프리터와 컴파일러를 같이 사용한다. ([CPython](https://en.wikipedia.org/wiki/CPython)의 경우)

**Python 소스코드** `->(컴파일러)` **bytecode** `->(인터프리터)` **기계어** 

--- 

![Python Interpreter](https://www.c-sharpcorner.com/article/why-learn-python-an-introduction-to-python/Images/last2.png)
> [https://www.c-sharpcorner.com/article/why-learn-python-an-introduction-to-python/](https://www.c-sharpcorner.com/article/why-learn-python-an-introduction-to-python/)

Java 설명과 동일하게 Python 언어를 해석할 수 있는 Python 해석기(그림에서 Python Interpreter)를 설치한다는 표현이 더 맞다.

## Node.js 설치
Node.js의 경우 Java, Python과 다르게 **Node.js 설치**라는 말이 이상하지 않다.

Node.js는 프로그래밍 언어가 아니고 JavaScript 런타임이다.
가끔 Node.js를 처음 접하는 개발자들은 Node.js가 언어라고 잘못 알기도 한다.
> Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
> [https://nodejs.org/en](https://nodejs.org/en)

Node.js는 JavaScript를 실행시킬 수 있는 런타임 즉, 소프트웨어고 이 소프트웨어에서 실행되는 프로그램 언어가 JavaScript인 것이다.

그래서 그 누구도 JavaScirpt를 설치한다고 말하지 않는다.

---

![JavaScript engine v8](https://marcradziwill.com/static/5c9eaa8964c688cea69db26a1da0f11e/eea4a/V8-pipeline.jpg)
>https://marcradziwill.com/blog/mastering-javascript-high-performance

## C 설치
C의 경우도 조금 다르다. 대부분의 개발자는 **C 설치**를 검색하지 않는다.
[C 설치](https://www.google.com/search?q=C%20%EC%84%A4%EC%B9%98)를 보더라도 대부분 Visual Studio를 설치하는 내용들이 검색된다.

학부 수업에서도 C 언어를 코딩하기 전에 Visual Studio를 먼저 설치했던 기억이 난다.

C 언어는 순수한 컴파일 언어라고 할 수 있다. 고급어인 C 언어는 컴파일러를 통해 기계어로 변환되고 컴퓨터는 이를 해석한다.

Visual Studio가 컴파일러를 포함한 소프트웨어이기 때문에 C 언어로 작성된 소스코드를 컴퓨터가 해석할 수 있는 기계어로 변환하기 위해 Visual Studio를 설치하는 것이다. (리눅스에선 [gcc]([https://namu.wiki/w/GCC](https://namu.wiki/w/GCC)))

---

![c compile](https://miro.medium.com/max/1036/1*wHKe6W4opLmk6pb7sxZz6w.png)
> [https://medium.com/@laura.derohan/compiling-c-files-with-gcc-step-by-step-8e78318052](https://medium.com/@laura.derohan/compiling-c-files-with-gcc-step-by-step-8e78318052)


### Conclusion
별거 아니라고 생각할 수 있는 주제일 수 있다. 그리고  한 번도 생각해보지도 않은 그리고 않을 주제일 수도 있다.
또한 누군가에겐 생각해볼 가치도 없는 주제일 수 있다.

하지만, 필자의 경우 평소 당연하게 생각했던 **Java 설치**라는 말 자체에 의문이 생겨 깊게 알아보았다.
이 과정에서 사용했었던 프로그래밍 언어들의 동작 원리에 대해서 다시 자세히 알아본 기회가 됐다.

누군가가 **Java 설치**라고 말한다 해서 `"그건 잘못된 말이야! 생각해봐 어떻게 프로그래밍 언어를 설치해! 안 그래?"`라고 하나하나 꼬투리 잡진 않을 것이다.

**알고 하지 않는 것과 모르기 때문에 못하는 것은 다르다.**
