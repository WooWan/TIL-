# React native 동작 방식

## 어떻게 JavaScript 코드를 IOS, Android 이해할 수 있을까?

IOS 는 JavaScriptCore 엔진(애플이 개발한 Chrome V8와 유사한 엔진)을 내장하고 있고, 안드로이드는 이를 내장하지 않고 있다.

그래서 React Native는 JavaScriptCore 엔진을 Android에 함께 제공한다.



### React Native Bridge

따라서, 기본적으로 JavaScript를 이해할 수 없기 때문에 JSON 포맷 데이터로 간접 통신을 한다. 이 과정을 React Native Bridge에서 처리한다.

![img](https://miro.medium.com/v2/resize:fit:1400/1*FLWFhI5iKhOAeRDOpywLdw.png)



### Build time

RN 프로젝트를 생성하면, IOS, Android 플랫폼을 지원하기 위한 Java, Object-c 코드를 포함하고 있다.

빌드 단계에서, JavaScript 코드는 Metro에 의해 번들링되고, Java와 C 바이너리 파일을 생성한다.

번들링된 JavaScript 파일과 바이너리 파일을 이용하여 실행 가능한(apk, IPA) 파일을 생성한다.



### Rum time

APK 파일(for Android)는 JavaScript를 실행하기 위한 JavaScript Core엔진을 포함하고 있다. 따라서 런타임 때, JS Core엔진 위에서 JavaScript가 실행되고, JS Core가 메세지들을 생성하고 이를 Bridge에 전달한다.

Bridge는 메세지들을 직렬화하고, 이를 JSON 형태로 직렬화한다.

네이티브 코드는 이 메세지들을 역직렬화하여(deserialize) 작업들을 수행한다.





 Bridge에 전달하여 native platform에 그린다.



### React Native Threads

어플리케이션을 실행하면 3개의 쓰레드가 실행된다.

* Main thread: 유저와의 상호작용을 담당과 UI 렌더링을 담당
* JavaScript Trhead: 비즈니스 로직 ㄷ마당
* Shadow Thread: 레이아웃 구성과 위치를 계산을 담당(Yoga engined: flex 스타일을 native로 변환)



### Bridge의 동작 과정

<img src="https://miro.medium.com/v2/resize:fit:1400/1*XS4xzTNoDu2voJXpirnQwA.png" alt="img" style="zoom:67%;" />

1. 사용자가 터치 또는 스크롤 이벤트를 발생시킨다.
2. 직렬화된 메시지는 필요한 모든 데이터와 함께 브릿지를 통해 네이티브 측에서 전송됩니다.
3. 자바스크립트는 메시지를 수신하고 역직렬화하여 다음에 수행할 작업을 결정합니다.(아이콘을 변경!)
4. 메시지는 요청된 작업에 대한 정보와 함께 브릿지를 통해 자바스크립트 계층에서 전송됩니다.
5. 네이티브 측에서 메시지를 수신하고 역직렬화하여 뷰를 업데이트합니다.



### bridge의 문제점

bridge는 비동기적으로 동작한다. 따라서 연속적인 입력 작업에서 UI 불일치 문제(Jump) 가 발생할 수 있다.

1. 비동기 처리로 인한 UI 렌더링 지연(UI 불일치- jump)
2. 직렬화와 역직렬화로 인한 오버헤드



### JSI - bridge의 새로운 대안

`bridge`를 통해 메세지를 주고 받는 대신에, 직접적으로 native API를 호출할 수 있는 인터페이스

#### 특징

* 동기적
* Suspense를 활용 가능



