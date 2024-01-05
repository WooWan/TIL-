#  ReScript 공식문서 Variant 정리

* ReScript 공식 문서를 읽으며 필요한 내용 정리한 노트
* [공식 문서](https://rescript-lang.org/docs/manual/latest/variant)





## Variant

타입 합집합 표현

```ts
// "this or that".
type myResponse =
  | Yes
  | No
  | PrettyMuch

let areYouCrushingIt = Yes
```

