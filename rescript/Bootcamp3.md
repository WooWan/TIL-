# 부트캠프 3일차

- AOC 코드는 [여기](https://github.com/WooWan/bootcamp-aoc/blob/main/rescript/src/Week1/Year2020Day6.res)에서 확인할 수 있습니다!

## 오늘 한 일

- [x] Day6 풀이, Day3, Day5 리팩토링
- [x] 함수형 프로그래밍 개념 학습 중!

📚

## 배운 점

- 아직 자바스크립트에서는 intersect, union이 proposal 단계인데, rescript에서는 built-in으로 지원한다!!

- map, filter, reduce등 에서 index를 같이 넘기는 것을 지양하자

  => 함수형 프로그래밍에서 되도록 요소를 이용해야 되고(복잡성 증가, side effect?)

- 목적이 명확한 함수를 작성하고, 이러한 함수들을 합성하여 문제를 해결하자!

## 공부한 내용 정리

### 왜 rescript 배열에서 data-first 접근법을 사용할까?

```javascript
//rescript 배열에서 동일한 타입만을 넣을 수 있다.
let aList = [2, "a"];

//마찬가지로, rescript 컴파일러는 n => n+1을 보고 int로 추론하지만, 문자열이 들어와서 에러를 발생시킨다.
let words = ["hi"]
let res = List.map(n => n + 1, words)

//컴파일러 타입 추론과 같은 접근 법을 사용한다.
let ages = Belt.List.map(User.admins, u => u.age 또는 u.name)
```

- 복잡한 유즈케이스에서는 데이터 흐름을 파악에 data-first 접근법이 디버깅에 용이할 것 같다.

  - `Belt`는 기본적으로 data-first 접근 방법을 사용하고 있다. (Js.Array는 data-last)

  - `|>`연산자는 추가적으로 함수 호출을 하는 데 비해, `->` 연산자는 추가 함수 호출 없이 바로 실행된다.
  - `a(b)` 와 `b -> a` 동일

Reference https://www.javierchavarri.com/data-first-and-data-last-a-comparison/

### pattern-matching

처음 봤을 때, TypeScript의 유니온 타입과 비슷하다고 생각하였는데, edge case를 방지할 수 있다는 점에서 더 강력한 타입을 지원하는 것 같다.

### Variant

추가 공부 필요

## 느낀 점

rescript과 functional programming이 처음이어서 코드를 작성하는 게 낯설었는데 손에 익어가고 있다.

## 참고 자료

https://www.javierchavarri.com/data-first-and-data-last-a-comparison/
