# 부트캠프 8일차

## 오늘 한 일

- [x]  Day4, Day3를 피드백 내용을 반영하여 리팩토링 진행했습니다.
- [x] 리스크립트 Function, Option, Variant, genType 다시 읽고 정리하였습니다.



## ReScript 공식문서 Function 정리

* ReScript 공식 문서를 읽으며 필요한 내용 정리한 노트

* [공식 문서](https://rescript-lang.org/docs/manual/latest/function)

  

## Function

### Labeled Arguments

함수 호출 시에, 인자를 라벨링하여 호출할 수 있다.

```ts
let addCoordinates = (~x, ~y) => {
  // use x and y here
}
// ...
addCoordinates(~x=5, ~y=6)

```



### Explicitly Passed Optional

```ts
// before
let result =
  switch payloadRadius {
  | None => drawCircle(~color)
  | Some(r) => drawCircle(~color, ~radius=r)
  }

// after
// ?variable로 optional parameter를 전달할 수 있다.
let result = drawCircle(~color, ~radius=?payloadRadius)


```



### Partial Application

`...`를 통해  함수 인자를 부분 적용할 수 있다.

```ts
let add = (a, b) => a + b
let addFive = add(5, ...)
```



### Error handling

명시적으로 switch문을 통해서 에러를 핸들링 할 수 있다.

```ts
exception SomeReScriptException

let somethingThatMightThrow = async () => raise(SomeReScriptException)

let someAsyncFn = async () => {
  switch await somethingThatMightThrow() {
  | data => Some(data)
  | exception JsError(_) => None
  | exception SomeReScriptException => None
  }
}
```



## ReScript 공식문서 Option 정리

* ReScript 공식 문서를 읽으며 필요한 내용 정리한 노트
* [공식 문서](https://rescript-lang.org/docs/manual/latest/null-undefined-option)



## Null, Undefined and Option

>ReScript itself doesn't have the notion of `null` or `undefined`



* Option은 variant의 일종이다.



## Interoperate with JavaScript `undefined` and `null`

Some의 컴파일

```ts
//rescript
let x = Some(5)

//js
var x = 5;
```



None의 컴파일

```ts
//rescript
//None은 undefined로 컴파일된다.
let x = None

//js
var x;
```



### 주의점

```ts
//rescript
//절대 nested option 사용 금지
let x = Some(None)

//js
var Caml_option = require("./stdlib/caml_option.js");

var x = Caml_option.some(undefined);
```



* Undefined => `option`

* Nullable => Null
