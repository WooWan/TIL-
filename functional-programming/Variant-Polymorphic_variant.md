# Variant와 Polymorphic Variant



## Variant

* Nominally typed(명시적 타입)





## Polymorphic Variant

#### Strucutally typed(구조적 타이핑)

Variant는 명시적 타이핑을 지원해서, 아래 코드에서 에러를 발생시킬 것이다. 하지만 구조적 타이핑 시스템에서는 displayColor의 부분 타입이기 때문에 에러가 발생하지 않는다.

```ts
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



#### Extra Constraints on Types

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



#### Coercion(타입 변환)

You can convert a poly variant to a `string` or `int` at no cost:

```ts
type company = [#Apple | #Facebook]
let theCompany: company = #Apple

let message = "Hello " ++ (theCompany :> string)
```





## 차이점

