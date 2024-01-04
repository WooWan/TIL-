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
