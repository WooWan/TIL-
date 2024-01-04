# 부트캠프 7일차

## 오늘 한 일

- [x] [Parse, don't validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/) 를 다시 읽고 day4 리팩토링에 적용하였습니다.
- [x] day 4 리팩토링의 리팩토링을 진행했습니다 🫠
- [x] Variant와 PolyVariant 차이점에 대해 공식문서를 읽고 정리하였습니다.
- [x] Currying와 Partial application의 차이를 알아보고, 이에 대한 함수 시그니쳐 표현법을 작성하였습니다.
- [x] ReScript optional field 공식문서를 읽고 코드에 적용하였습니다.
- [ ] Functor와 Monad에 대해서 정리하기



## 학습 내용

### Currying과 Partial application

#### Variant

- Nominally typed(명시적 타입)

#### Polymorphic Variant

##### Structurally typed(구조적 타이핑)

Variant는 명시적 타이핑을 지원해서, 아래 코드에서 에러를 발생시킬 것이다. 하지만 구조적 타이핑 시스템에서는 displayColor의 부분 타입이기 때문에 에러가 발생하지 않는다.

```ts
//선언 방법
type preferredColors = [#white | #blue]

let myColor: preferredColors = #blue

let displayColor = v => {
  switch v {
  | #red => "Hello red"
  | #green => "Hello green"
  | #white => "Hey white!"
  | #blue => "Hey blue!"
  }
}

Js.log(displayColor(myColor))

```

##### Extra Constraints on Types

또한, lower bound, upper bound 타입을 지원한다.

```ts
RES
// Only #Red allowed. Closed.(Closed 타입)
let basic: [#Red] = #Red

// May contain #Red, or any other value. Open
// here, foreground will actually be inferred as [> #Red | #Green]
let foreground: [> #Red] = #Green

// The value must be, at most, one of #Red or #Blue
// Only #Red and #Blue are valid values
let background: [< #Red | #Blue] = #Red
```

##### Coercion(타입 변환)

You can convert a poly variant to a `string` or `int` at no cost:

```ts
type company = [#Apple | #Facebook]
let theCompany: company = #Apple

let message = "Hello " ++ (theCompany :> string)
```



#### 차이점

공식 문서에서는 대부분의 케이스에 Variant를 Polymorphic Variant보다 우선하여 사용하라고 작성되어 있다.



### Functor와 Monad

#### Functor

Functor란 Mappable한 구조체(map, then 등)

#### Monad

chaining과 flatten, 단위 함수를 지원하는(Wrapping) Functor

```js
let hello = "hello"
hello->Some
```



#### Functor law

* Functor law란 Functor를 예측 가능하게 만들어주는 법칙들

1. 항등 법칙:mapping을 할 때, 동일한 결과물을 반환한다.

   ```js
   const array = [1, 2, 3];
   const identity = x => x;
   
   console.log(array.map(identity));  // 결과: [1, 2, 3]
   ```

2. 합성 법칙:  두 함수의 합성과 순차적으로 함수를 적용하는 것은 동일한 결과를 반환한다.

   ```js
   const array = [1, 2, 3];
   const addOne = x => x + 1;
   const multiplyByTwo = x => x * 2;
   
   const composition = array.map(x => multiplyByTwo(addOne(x)));
   const sequential = array.map(addOne).map(multiplyByTwo);
   
   console.log(composition);  // 결과: [4, 6, 8]
   console.log(sequential);   // 결과: [4, 6, 8]
   ```



## 느낀 점

함수형 프로그래밍에 점차 익숙해고 있다. 

리스크립트의 특성과 지원하는 여러 기능을 알아가는 것이 시작이다

 