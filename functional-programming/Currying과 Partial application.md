# Currying과 Partial application



## Currying이란?

함수가 여러 개의 인자를 받는 대신, 나머지 인자를 받기 위한 새로운 함수를 반환하는 함수를 의미한다.

```js
// f(x,y) = h(x)(y) = x + y 라는 수학적 표현을 커링으로 나타내면,
add(x,y) = add(x)(y)로 변환할 수 있다.

// Arrow function
const add = x => y => x + y;

// Normal function
function add(x) {
  return function(y) {
    return x + y;
  }
}
```





### Partial application

Partial application은 currying과 비슷하게 보이지만, 여러 인자를 고정하고 다른 인자를 받아서 사용할 때 주로 사용한다.  즉, 여러 개의 인자를 한 함수에서 받을 수 있다는 차이점이 있다.

아래 예제에서는 a,b를 고정하고, c라는 인자를 받아 사용한다.

```ts
// 원래의 함수
function sum(a: number, b: number, c: number): number {
  return a + b + c;
}

// Partial Application을 사용한 함수 정의
function partiallyApplySum(a: number, b: number): (number) => number {
  return function(c: number): number {
    return sum(a, b, c);
  };
}

// 사용 예
const addFiveAndThree = partiallyApplySum(5, 3); // 5와 3을 더하는 함수 반환
console.log(addFiveAndThree(10)); // 출력: 18

```

