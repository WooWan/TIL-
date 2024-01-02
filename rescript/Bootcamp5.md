# 부트캠프 5일차



## 오늘 한 일
- [x] Day2 리팩토링
- [x] Day4 풀이
- [x] Parse, don't validate 정리
- [ ] Variant vs Polymorphic Variant 차이, Currying vs Partial application 차이 이해



## 학습 내용

### Variant vs Polymorphic Variant

Variant는 Nominal type(명시적 타입), Polymorphic Variant(구조적 타이핑)을 지원한다.

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

type preferredColorsV2 =
  | White
  | Blue

let myColorV2: preferredColorsV2 = Blue

//error Nominal 타입으로, White, Blue 타입으로 추론한다.
let displayColorV2 = v => {
  switch v {
  | White => "Hey white!"
  | Blue => "Hey blue!"
  | Black => "Hey black!"
  }
}

```



### Currying vs Partial application

#### Currying

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



#### Partial appliation

Partial application은 currying과 비슷하게 보이지만, 여러 인자를 고정하고 다른 인자를 받아서 사용할 때 주로 사용한다.  즉, 여러 개의 인자를 한 함수에서 받을 수 있다는 차이점이 있다.

```ts
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



### Parse, don't validate

Validation을 활용하면 언어의 타입 추론 기능을 완전히 활용하지 못한다.

TypeScript 예시

```ts
//Validation => 타입 추론 불가
function validateEmails(emails: string[]): string[] {
    const validEmails: string[] = [];
    for (const email of emails) {
        // 단순한 예시로, 이메일에 '@' 문자가 포함되어 있는지 확인합니다.
        if (email.includes('@')) {
            validEmails.push(email);
        } else {
            console.log(`Invalid email found: ${email}`);
        }
    }
    return validEmails;
}

const emails = ["example@test.com", "invalidemail", "another@test.com"];
const validEmails = validateEmails(emails);
// 'validEmails'는 검증된 이메일의 문자열 배열입니다.

//Parsing 접근 법
class ValidEmail {
    public readonly email: string;

    constructor(email: string) {
        if (email.includes('@')) {
            this.email = email;
        } else {
            throw new Error(`Invalid email: ${email}`);
        }
    }
}

function parseEmails(emails: string[]): ValidEmail[] {
    const validEmails: ValidEmail[] = [];
    for (const email of emails) {
        try {
            validEmails.push(new ValidEmail(email));
        } catch (error) {
            console.error(error);
        }
    }
    return validEmails;
}

const emails = ["example@test.com", "invalidemail", "another@test.com"];
const validEmails = parseEmails(emails); // 'validEmails'는 'ValidEmail' 인스턴스의 배열.

```



#### 차이점

Currying은 함수 인자를 한 개씩 고정하는 반면, Partial application은 여러 개의 인자를 고정할 수 있다.



## TODO

* Parse, don't validate 다시 읽어보기
* Day 3, 4 리팩토링 

