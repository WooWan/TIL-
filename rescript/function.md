# ReScript 공식문서 Function 정리

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

