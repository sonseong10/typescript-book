# Never

> [Youtube: Never 타입에 대한 동영상 강의](https://www.youtube.com/watch?v=aldIFYWu6xc)

> [Egghead: Never 타입에 대한 동영상 강의](https://egghead.io/lessons/typescript-use-the-never-type-to-avoid-code-with-dead-ends-using-typescript)

프로그래밍 언어 디자인에는 _코드 흐름 분석_ 을 수행할 때 **자연스럽게** 결과로 나오는 _바닥_ 타입이라는 개념이 있습니다. TypeScript도 _코드 흐름 분석_ 을 하기 때문에 (😎) 발생 가능성이 없는 것들을 표현할 방법이 필요합니다.

TypeScript가 이 _바닥_ 타입을 나타내기 위해 사용하는 것이 `never` 타입입니다. 이 타입이 발생하는 경우들:

- 리턴하지 않는 함수 (e.g. 함수 내용에 `while(true){}`가 들어 있는 경우)
- 항상 예외를 던지는 함수 (e.g. `function foo(){throw new Error('Not Implemented')}` 에서 `foo`의 리턴 타입이 `never`)

당연히 이 어노테이션을 프로그래머가 직접 사용할 수도 있습니다

```ts
let foo: never; // 오케이
```

그렇지만, _`never` 타입에는 다른 never 타입만_ 할당 가능합니다. 예를 들면:

```ts
let foo: never = 123; // 오류: number 타입은 never에 할당할 수 없음

// 오케이, 함수의 리턴 타입이 `never`
let bar: never = (() => {
  throw new Error(`Throw my hands in the air like I just don't care`);
})();
```

좋습니다. 그러면 핵심 사용 방법으로 넘어가죠 :)

# 사용 사례: 빠짐없는 검사(Exhaustive Check)

발생 불가능한 상황(never 컨텍스트)에서 never 함수를 호출할 수 있습니다.

```ts
function foo(x: string | number): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }

  // never 타입이 없다면 오류 발생 :
  // - 코드 경로 중에 값을 반환하지 않는 경로 존재 (strictNullChecks)
  // - 또는 접근 불가능한 코드 검출
  // 하지만 TypeScript는 `fail` 함수는 `never` 리턴임을 알 수 있고
  // 런타임 안전 / 빠짐없는 검사를 위해 이런 함수를 호출할 수 있음.
  return fail("Unexhaustive!");
}

function fail(message: string): never {
  throw new Error(message);
}
```

그리고 `never`에는 `never`만 할당할 수 있기 때문에 *컴파일 시간*의 빠짐없는 검사 목적으로 사용할 수 있습니다. 이 내용은 [_구별된 유니온_ 단원](./discriminated-unions.md)에 나와 있습니다.

# `void`와 혼동

함수가 매끄럽게 종료하지 않을 때 `never`가 반환된다고 하면 직관적으로 이것은 `void` 같은 것이라고 생각하기 쉽습니다. 그렇지만 `void`는 하나의 단위이고 `never` 모순(falsum)입니다.

아무것도 *반환*하지 않는 함수는 단위 `void`를 반환하는 것입니다. 하지만 _영원히 리턴하지 않는_ 함수 (또는 항상 throw하는 함수)는 `never`입니다. `void`는 할당이 가능한 타입이지만 (`strictNullCheckings`를 끄면) `never`는 절대 `never` 이외에는 할당할 수 없습니다.

# never 반환 함수의 타입 추론

함수 선언시 TypeScript는 기본으로 `void`를 가정합니다, 아래 처럼:

```ts
// 추론된 리턴 타입: void
function failDeclaration(message: string) {
  throw new Error(message);
}

// 추론된 리턴 타입: never
const failExpression = function (message: string) {
  throw new Error(message);
};
```

물론 명시적인 어노테이션을 추가하면 고칠 수 있습니다:

```ts
function failDeclaration(message: string): never {
  throw new Error(message);
}
```

이렇게 하는 핵심 이유는 실제 JavaScript 코드와의 호환성 유지입니다:

```ts
class Base {
  overrideMe() {
    throw new Error("You forgot to override me!");
  }
}

class Derived extends Base {
  overrideMe() {
    // Code that actually returns here
  }
}
```

`Base.overrideMe` 호출의 경우.

> 실제 TypeScript에서는 `abstract` 함수를 써서 이 문제를 해결할 수 있지만 이 타입 추론은 호환성을 위해 유지되고 있습니다.

# Type inference in never returning functions

For function declarations TypeScript infers `void` by default as shown below:

```ts
// Inferred return type: void
function failDeclaration(message: string) {
  throw new Error(message);
}

// Inferred return type: never
const failExpression = function (message: string) {
  throw new Error(message);
};
```

Ofcourse you can fix it by an explict annotation:

```ts
function failDeclaration(message: string): never {
  throw new Error(message);
}
```

Key reason is backword compatability with real world JavaScript code:

```ts
class Base {
  overrideMe() {
    throw new Error("You forgot to override me!");
  }
}

class Derived extends Base {
  overrideMe() {
    // Code that actually returns here
  }
}
```

If `Base.overrideMe` .

> Real world TypeScript can overcome this with `abstract` functions but this inferrence is maintained for compatability.

<!--
PR: https://github.com/Microsoft/TypeScript/pull/8652
Issue : https://github.com/Microsoft/TypeScript/issues/3076
Concept : https://en.wikipedia.org/wiki/Bottom_type
-->
