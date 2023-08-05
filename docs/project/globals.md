# global.d.ts

우리는 전역 vs 파일을 언제 모듈화를 해야하는지에 대해서 논의했습니다.[projects](./modules.md) 파일을 기본으로 모듈을 사용하는 것을 추천합니다. 이것은 전역을 오염시키지 않습니다.

그렇기는하지만, 만약 당신이 타입스크립트 개발자로 시작한다면 `globals.d.ts`파일을 주고 싶습니다. 이것은 interfaces / types 를 전역 네임스페이스로 어떤 타입이든 쉽게 생성하도록 해주고 타입스크립트 코드에서 타입을 쉽게 사용할 수 있도록 해줍니다.

Another use case for a `global.d.ts` file is to declare compile-time constants that are being injected into the source code by Webpack via the standard [DefinePlugin](https://webpack.js.org/plugins/define-plugin/) plugin.

```ts
declare const BUILD_MODE_PRODUCTION: boolean; // can be used for conditional compiling
declare const BUILD_VERSION: string;
```

> For any code that is going to generate _JavaScript_ we highly recommend using _file modules_, and only use `global.d.ts` to declare compile-time constants and/or to extend standard type declarations declared in `lib.d.ts`.

- Bonus: The `global.d.ts` file is also good for quick `declare module "some-library-you-dont-care-to-get-defs-for";` when doing JS to TS migrations.
