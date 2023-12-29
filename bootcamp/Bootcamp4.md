# 부트캠프 4일차



## 오늘 한 일

- [x] Day6 풀이, Day3, Day5 리팩토링



## 학습 내용

이전까지는 먼저 코드를 작성하여 문제 풀이를 진행하였는데, 오늘은 module를 사용하여 문제풀이를 진행했습니다

module을 사용하는 과정에서 여러 궁금점이 생겼는데

1. type concat (Intersection type)

```ts
//ts의 경우 `&` 또는 `extends`로 interesction type을 사용하는데, rescript에서는 어떻게 타입을 합칠 수 있을지 조사 필요
type condition = (string, string, string)

// type concat 방법?
// type parsedLine = condition & string
// type parsedLine = (condition, string)
type parsedLine = (string, string, string, string)
```



2. Union type

   validator를 callback으로 사용하였는데 type 선언하는데 어려움을 겪었다. 

   코드 가독성을 위해 callback 중첩은 지양하는 것이 좋을 것 같다.

   ```ts
   //before
   let countValidPasswords = validator => {
     parseInput()
     ->Belt.Array.map(validator)
     ->Belt.Array.reduce(0, (acc, x) => acc + (x ? 1 : 0))
     ->Js.log
   }
   
   countValidPasswords(chunk => validatePassword(chunk, generateRangeRegex))
   countValidPasswords(validatePasswordV2)
   
   //이런 식으로 할 수 있으면 될 것 같은데..
   type Validator = A | B
   let countValidPasswords = (Validator) => unit
   
   
   ```

3. Day3 리팩토링

   이전에는, index를 이용하여 `나무`인지 판별하였는데, FP에서는 element외에 참조를 지양해야한다. (복잡성, index 의존)

   ```ts
   //before
   let countTrees = (grid, dir) => {
     grid
     ->Belt.Array.concatMany
     ->Belt.Array.keepWithIndex((_, index) => isTrack(grid, dir, index))
     ->Belt.Array.map(x => x == "#" ? 1 : 0)
     ->Belt.Array.reduce(0, (acc, val) => acc + val)
     
   //after
   let countTrees = ((right, down)) => {
     let rec countSlope = (row, col, count) => {
       let isBounded = row >= height
   
       switch isBounded {
       | true => count
       | false => {
           let updatedCount = inputs[row][col] == "#" ? count + 1 : count
           countSlope(row + down, mod(col + right, width), updatedCount)
         }
       }
     }
     countSlope(0, 0, 0)
   }
   ```



## ToDo

- [ ] Day2 리팩토링 
- [ ] Currying, Partial 차이점
- [ ] Variant, Polymorphic Variant 차이점 조사!!
