# Browser window.alert()

https://developer.mozilla.org/ko/docs/Web/API/Window/alert

### alert() 의 특징

>  대화 상자는 modal window(부모 창으로 돌아가기 전에 사용자의 상호 작용을 요구하는 자식 창)입니다. 따라서 대화 상자는 그 대화 상자가 닫힐 때까지 그 사용자가 그 대화 상자를 제외한 환경(interface), 즉 그 프로그램(웹 브라우저 등)의 나머지 환경에 접근할 수 없게 만듭니다. 이러한 이유 때문에 당신은 대화 상자( 또는 modal window)를 만드는 함수를 남용하면 안 됩니다.

> **Note:** The alert box takes the focus away from the current window, and forces the browser to read the message. Do not overuse this method, as it prevents the user from accessing other parts of the page until the box is closed.

<br>

### alert() 브라우저별 차이

#### Chrome
alert() 이 발생하면 현재 창의 focus를 alert() 발생 창으로 이동시킨다.
또한 최소화된 창에서도 alert() 발생을 알리기 위해 사용자에게 표시해준다.
Windows OS 기준으로 하단에 실행창이 깜빡거린다.

#### IE
alert() 이 발생해도 focus 이동시키지 않는다. 사용자는 어떤 창에서 alert() 이 발생했는지 찾아야 한다.

<br>

### Conclusion
alert()은 window 객체의 함수이고 동기적으로 실행된다.
또한, 각 브라우저 엔진은 alert()이 발생하면 alert()을 제외한 다른 브라우저 환경에 접근할 수 없게 막는다.
JS는 싱글 쓰레드 비동기이기 때문에 alert() 같은 함수는 JS 성능을 안 좋게 할 수 있다.
최대한 적게 쓰는 것이 좋다.
