# 부트캠프 12일차

## 오늘 한 일

- [x] 리스크립트 과제 피드백을 기반으로 리팩토링을 진행하였습니다.
- [x] ReScript binding 사용법과 JS 컴파일 파일을 보면서 작동 원리를 공부하였습니다.



## 학습 내용

리스크립트는 자바스크립트와의 호환성을 위해 binding을 지원한다.

```tsx
// .res 리액트 컴포넌트는 아래 .js로 컴파일된다.
// @react.component 는 컴파일러가 리액트 컴포넌트 형식으로 컴파일 할 수 있도록 도와주는 directive 
// => function Feeds(props) 형태로 변환
function Feeds(props){
  //...
}

var make = Feeds;

export {
  make ,
}

```



* @react.component 는 컴파일러가 리액트 컴포넌트 형식으로 컴파일 할 수 있도록 도와주는 directive

### Raw JavaScript in ReScript

* %%raw 는 ReScript은  JavaScript 문자열을 가져와서 출력에 그대로 붙여넣는다.

* `%raw` lets you embed expression-level JS code (표현식만 가능, 표현식이 아닌, 변수, class등은 컴파일 에러)

```ts
// 정상 작동
let add = %raw(`
  function(a, b) {
    return a + b
  }
`)

//안됨!
let add = %raw(`
  let b  = 3;
  function(a) {
    return a + b
  }
`)

//compile
var add = (let b  = 3;
  function(a) {
    return a + b
  });
```





## 오늘의 삽질 🌱 

ReScript binding은 아직 익숙하지 않아서 아래 코드를 export하기 위해서 몇 시간 동안 짱구를 굴렸지만 실패.. 

아래  raw 자바스크립트 코드를 export하여 사용하려고 시도했지만, %raw는 표현식에만 사용할 수 있고,  %%raw를 사용하여 export하는 방식은 지원하지 않는 것으로 보인다.

따라서, js module을 export해야할 때, 외부에 js 모듈을 두고, external과  @module을 사용하여 export하는 것이 심신에 편하다

```js
%%raw(`
import React from "react";
class ErrorBoundaryWithRetry extends React.Component {
  constructor(props) {
    super(props);
    this.state = { error: null, fetchKey: 0 };
    this._retry = this._retry.bind(this);
  }

  static getDerivedStateFromError(error) {
    return { error, fetchKey: 0 };
  }

  _retry() {
    this.setState(prevState => ({
      error: null,
      fetchKey: prevState.fetchKey + 1
    }));
  }

  render() {
    const { children, fallback } = this.props;
    const { error, fetchKey } = this.state;
    if (error) {
      return typeof fallback === "function"
        ? fallback({ error, fetchKey, retry: this._retry })
        : fallback;
    }
    return children({ fetchKey });
  }
}
// export default ErrorBoundaryWithRetry;
`)

@react.component
external make: (
  ~children: state => React.element,
  ~fallback: params<Js.Exn.t> => React.element,
) => React.element = "ErrorBoundaryWithRetry"
```