# 타입스크립트 모듈 해상도

TypeScript의 모듈 해상도는 실제 모듈 시스템/로더를 모델링하고 지원하려고 시도합니다(commonjs/nodejs, amd/requirejs, ES6/systemjs 등). 가장 간단한 조회는 상대 파일 경로 조회입니다. 그 이후에는 *다양한 모듈 로더에 의해 수행되는 마법의 모듈 로딩의 특성으로 인해* 조금 복잡해집니다.

## 파일 확장자

foo` 또는 './foo`와 같은 모듈을 가져옵니다. 모든 파일 경로 조회에 대해 TypeScript는 컨텍스트에 따라 올바른 순서로 `.ts` 또는 `.d.ts` 또는 `.tsx` 또는 `.js`(선택 사항) 또는 `.jsx`(선택 사항) 파일을 자동으로 확인합니다. 모듈 이름과 함께 파일 확장자를 제공하면 안 됩니다(`foo.ts` 없이 `foo`만 제공).

## 상대 파일 모듈

상대 경로를 사용하는 가져오기(예:

```ts
import foo = require('./foo');
```

현재 파일에 대한 상대 위치(예: `./foo.ts`)에서 타입스크립트 파일을 찾도록 타입스크립트 컴파일러에 지시합니다. 이런 종류의 임포트에는 더 이상의 마법은 없습니다. 물론 디스크에서 익숙한 다른 *상대 경로*와 마찬가지로 `./foo/bar/bas` 또는 `../../../foo/bar/bas`와 같이 더 긴 경로가 될 수도 있습니다.

## 명명된 모듈

다음 문장을 사용합니다:

```ts
import foo = require('foo');
```

타입스크립트 컴파일러에 다음 순서로 외부 모듈을 찾으라고 지시합니다:

* 이미 컴파일 컨텍스트에 있는 파일에서 명명된 [module declaration](#module-declaration).
* 여전히 해결되지 않고 `--module commonjs`로 컴파일 중이거나 `--moduleResolution 노드`를 설정한 경우, [*node modules*](#node-modules) 해결 알고리즘을 사용하여 조회합니다.
* 여전히 해결되지 않고 `baseUrl`(및 선택적으로 `경로`)를 제공한 경우 [*path substitutions*](#path-substitutions) 해결 알고리즘이 작동합니다.

"foo"`는 더 긴 경로 문자열(예: "foo/bar/bas"`)일 수 있다는 점에 유의하세요. 여기서 핵심은 `./` 또는 `../`*로 시작하지 않는다는 것입니다.

## 모듈 선언

모듈 선언은 다음과 같습니다:

```ts
선언 모듈 "foo" {
    /// 일부 변수 선언
    export var bar:number; /*sample*/
}
```

이렇게 하면 모듈 `"foo"`, *임포트 가능*이 됩니다.

## 노드 모듈
노드 모듈 해상도는 실제로 Node.js/NPM에서 사용하는 것과 거의 동일합니다([공식 nodejs 문서](https://nodejs.org/api/modules.html#modules_all_together)). 다음은 제가 가지고 있는 간단한 멘탈 모델입니다:

* 모듈 `foo/bar`는 어떤 파일로 해석됩니다 : `node_modules/foo`(해당 모듈) + `foo/bar`

## 경로 치환

TODO.

[//Comment1]:https://github.com/Microsoft/TypeScript/issues/2338
[//Comment2]:https://github.com/Microsoft/TypeScript/issues/5039
[//Comment3ExampleRedirectOfPackageJson]:https://github.com/Microsoft/TypeScript/issues/8528#issuecomment-219172026
[//Coment4ModuleResolutionInHandbook]:https://github.com/Microsoft/TypeScript-Handbook/blob/release-2.0/pages/Module%20Resolution.md#base-url
