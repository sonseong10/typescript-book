# JSX Support

[![DesignTSX](https://raw.githubusercontent.com/basarat/typescript-book/master/images/designtsx-banner.png)](https://designtsx.com)

typeScript는 JSX 트랜슬레이션 및 코드 분석을 지원합니다. JSX에 익숙하지 않으신 분들을 위해 [official website](https://facebook.github.io/jsx/):

> JSX는 정의된 의미론이 없는 ECMAScript에 대한 XML과 유사한 구문 확장입니다. 엔진이나 브라우저에서 구현하기 위한 것이 아닙니다. JSX를 ECMAScript 사양 자체에 통합하자는 제안이 아닙니다. 다양한 전처리기(트랜스파일러)에서 이러한 토큰을 표준 ECMAScript로 변환하는 데 사용하기 위한 것입니다.

JSX의 동기는 사용자가 *자바스크립트*로 뷰와 같은 HTML을 작성할 수 있도록 하기 위해서입니다:

* 자바스크립트를 검사할 코드와 동일한 코드가 뷰 유형을 검사하도록 하세요.
* 뷰가 작동할 컨텍스트를 인식하도록 합니다(즉, 기존 MVC에서 *컨트롤러-뷰* 연결을 강화합니다).
* 새로운(아마도 잘못 입력된) 대안을 만드는 대신 `Array.prototype.map`, `?:`, `switch` 등과 같은 HTML 유지보수를 위해 자바스크립트 패턴을 재사용하세요.

이렇게 하면 오류 발생 가능성이 줄어들고 사용자 인터페이스의 유지보수 가능성이 높아집니다. 현재 JSX의 주요 소비자는 다음과 같습니다. [ReactJS from facebook](http://facebook.github.io/react/). 여기서 설명할 JSX의 사용법은 다음과 같습니다. 
