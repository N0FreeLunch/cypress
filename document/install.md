## NodeJS 버전

```sh
nvm use v18.15.0
```

## cypress 설치

```
yarn add cypress --dev
```

## cypress 실행하기

```
npx cypress open
```

- E2E 테스트와 Component TEST 항목이 나온다.
- E2E 테스트를 먼저 사용해 보도록 하자.
- E2E 테스트를 할 때는 브라우저를 선택하는 메뉴가 나온다.

## 브라우저의 실행

- cypress를 사용하기 위해서는 브라우저를 실행해야 한다. cypress에서 제공하는 테스트 대상 브라우저는 크롬과 엣지이다. 브라우저가 제공하는 기능을 이용해서 브라우저의 화면을 직접 실행하지 않고 코드를 통해서 브라우저의 화면을 시뮬레이션 할 수 있다.
- 'Start E2E Testing in Chrome'이란 버튼으로 E2E 테스트를 진행하도록 한다. 그럼 브라우저에 'http://localhost:54150/\_\_/#/specs'와 같은 주소의 페이지가 나온다. 이 페이지는 테스트를 돕는 다양한 UI를 제공한다.
