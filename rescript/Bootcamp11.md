# Bootcamp 11일차

과제 리스크립트 재작성은 [레포](https://github.com/WooWan/onboarding-resript)에서 확인할 수 있습니다.

## DONE!

* TypeScript로 작성한 과제를 ReScript로 재작성하였습니다.
* ReScript와 Relay를 프로젝트에 적용해보면서 익숙해지는 시간을 가졌습니다.



## 학습내용

## switch 문에서의 if/when clause

ReScript의 패턴 매칭은 모든 타입에 대해서 타입 안정성이 보장된다.

```ts
//ts의 예시
type Animal = {
  type: "dog" | "cat" | "bird";
};

function getAnimalSound(animal: Animal): string | undefined {
  if (animal.type === "dog") {
    return "Woof!";
  } else if (animal.type === "cat") {
    return "Meow!";
  } else if (animal.type === "bird") {
    return "Tweet!";
  } else {
    // 이 부분이 빠지면, 새로운 'type'이 추가됐을 때 오류를 감지할 수 없음
    return undefined;
  }
}

//중간에 cat이 빠져도 타입 에러가 발생하지 않는다.
//또한, 새로운 타입이 추가되어도 타입 에러가 발생하지 않는다.
//ts의 예시
type Animal = {
  type: "dog" | "cat" | "bird";
};

function getAnimalSound(animal: Animal): string | undefined {
  if (animal.type === "dog") {
    return "Woof!";
  } else if (animal.type === "bird") {
    return "Tweet!";
  } else {
    // 이 부분이 빠지면, 새로운 'type'이 추가됐을 때 오류를 감지할 수 없음
    return undefined;
  }
}
```

이에 반해, rescript에서는 타입 완전성을 보장한다.

```ts
type animal = 
  | Dog(string) // 이름을 포함
  | Cat(string)
  | Bird(string)

let getAnimalSound = (animal: animal) => {
  switch animal {
  | Dog(_) => "Woof!"
  | Cat(_) => "Meow!"
  | Bird(_) => "Tweet!"
  }
}
```



하지만 Variant가 담고 있는 값에 관해서는 타입 완전성을 보장하지 못한다. 

아래 `getHeightV2`의 경우에, height 범위에 대해서 타입 완전성을 보장하지 못하고, 순서에 영향을 받게 된다.  

```ts
type height2 = Cm(int) | In(int)

let getHeights = height2 =>
  switch height2 {
  | Cm(height) => height
  | In(height) => height
  }

let getHeightV2 = height =>
  switch height {
  | Some(height) if height >= 150 && height <= 193 => Some(Cm(height))
  | Some(height) if height >= 59 && height <= 76 => Some(In(height))
  | _ => None
  }

//즉, 개발자가 실수로 범위를 잘못 체크했을 때, 컴파일 에러가 발생하지 않지만, 런타임에 우리의 예상과 다르게 동작한다.
let getHeightV2 = height =>
 switch height {
  | Some(height) if height >= -100 && height <= -999 => Some(Cm(height))
  | Some(height) if height >= 200 && height <= 150 => Some(In(height))
  | _ => None
  }
```



### GraphQL - Relay

* Graphql 에는 `Query`, `Mutation`, and `Subscription` 가 존재한다.



### Relay Caching

Relay Caching은 query based가 아닌, graph node에 따라 캐시가 된다.

>In contrast to most other systems, Relay’s caching is not based on queries, but on graph nodes



* ... on Actor 란? 



relay에서 component 렌더전에 relay 데이터 페칭?

### Preloaded query를 쓰면 장점이 많아보이는데 실무에서는 preloaded 쿼리 사용?



#### rescript binding의 동작 원리?



## TODO:

1. 피드백을 바탕으로 프로젝트 개선
   - [ ] fetchPolicy는 if/else 와 같다. 역할 분리
   - [ ] FeedsHeader와 Feed의 searchText 의존성 제거
   - [ ] url로 바로 접근 시, 주소 창에 반영
   - [ ] magic string 제거
   - [ ] useQueryParams 자바스크립트 작성 후, 리스크립트로 다시 작성해보기
   - [ ] 에러 바운더리 reset key 추가