# ReScript 공식문서 Option 정리

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