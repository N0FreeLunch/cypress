## 사이프레스의 기본 문법 이해하기

### 대상 테스트 파일

- cypress/e2e/1-getting-started/todo.cy.js

### 페이지 방문

- 기본적으로 어떤 사이트의 동작을 테스트하기 위해서는 사이트의 어떤 페이지에 접근을 한 이후에 가능하다.
- 브라우저에서 사이트에 접근할 때는 주소창에 직접 주소를 입력하거나, 링크를 타고 페이지를 넘어가거나, 즐겨찾기에 등록한 사이트 주소에 접근하는 방법을 사용한다. 이는 html이 화면에 랜더링 된 부분에서의 조작과는 별도의 기능으로 실행되는 것이다.
- cypress의 기본 테스트 동작은 페이지에 접속한 상태에서 랜더링된 페이지의 동작을 테스트한다. 웹 페이지에 접근하는 동작은 랜더링된 페이지의 동작을 테스트와 별도의 기능이므로 기본적인 테스트 동작을 수행하기 전에 웹 페이지에 접근을 해야한다는 동작을 만들어 주어야 한다. 이것이 `beforeEach` 함수이다.
- `beforeEach` 함수의 이름은 'before each test'로 테스트 동작을 수행하기 전에 실행되는 동작을 정의하는 부분이다. 웹 페이지에 접속하는 동작은 랜더링된 페이지 위에서 일어나는 동작이 아니며 페이지 위에서 일어나는 동작을 테스트하기 위해서는 먼저 페이지에 접속해야 하므로 페이지 위에서의 동작을 수행하기에 앞서 이 함수 안에 페이지 접속을 하는 동작을 정의한다.

```js
beforeEach(() => {
  // Cypress starts out with a blank slate for each test
  // so we must tell it to visit our website with the `cy.visit()` command.
  // Since we want to visit the same URL at the start of all our tests,
  // we include it in our beforeEach function so that it runs before each test
  cy.visit("https://example.cypress.io/todo");
});
```

- `beforeEach`의 문법은 `beforeEach(() => { /* 동작 */ })`, `beforeEach(function () { /* 동작 */ })` 등의 방식으로 정의할 수 있다.
- 사이프레스 쪽에서 예제 코드를 사용할 수 있는 웹 페이지를 제공한다. `https://example.cypress.io/todo`으로 접근하여 예제 테스트 코드의 동작을 확인한다.

### 예제 코드

```js
it("displays two todo items by default", () => {
  // We use the `cy.get()` command to get all elements that match the selector.
  // Then, we use `should` to assert that there are two matched items,
  // which are the two default items.
  cy.get(".todo-list li").should("have.length", 2);

  // We can go even further and check that the default todos each contain
  // the correct text. We use the `first` and `last` functions
  // to get just the first and last matched elements individually,
  // and then perform an assertion with `should`.
  cy.get(".todo-list li").first().should("have.text", "Pay electric bill");
  cy.get(".todo-list li").last().should("have.text", "Walk the dog");
});
```

#### syntax

- 문법은 `it("테스트 타이틀", ()=>{ /* 정의된 테스트 */ })`으로 되어 있다.

#### title

- 작성할 때 주의할 점은 타이틀에 맞는 테스트를 `{}` 안에 정의해 주어야 한다는 것이다. 한 단위의 기능이 이뤄진다는 것은 여러 테스트 코드의 순차적인 실행이 완료 되어야 하는 것이다. 단어와 단어가 모여서 문장이 되듯이, 동작과 동작이 모여 하나의 기능이 된다. 테스트 동작이 나열된 일련의 과정이 어떤 목적을 위해 수행되는지에 관한 타이틀을 적는다.
- `"displays two todo items by default"`은 어떤 기능을 테스트할지를 이름을 정한다. 사용하는 언어에 따라 한글로도, 영어로도, 일본어로 적어도 된다.

#### `cy.get(태그선택자)`

- `cy.get(".todo-list li")`는 페이지에 랜더링된 태그를 선택하겠다는 의미이다. 하나만 선택하는 것이 아니라 태그 선택자에 매칭되는 여러 개의 태그를 선택한다. 브라우저의 자바스크립트에서 동일한 태그를 선택하는 기능으로는 `document.querySelectorAll('.todo-list li')`이란 기능이 있다. 물론 `cy.get(태그선택자)`는 사이프레스에서 제공하는 기능으로 태그를 선택하고 실행하는 기능에 초점을 맞추는 것이며, `document.querySelectorAll`는 브라우저의 태그를 선택하고 변경하는데 초점이 맞춰진 기능이다.

#### should

- `cy.get(".todo-list li").should("have.length", 2);` 'should'는 '해야한다'라는 의미를 가진 조동사이다. 조동사는 동사와 함께 쓰인다.
- `should` 메소드에서 함께 사용할 수 있는 동사의 리스트를 [다음](https://docs.cypress.io/guides/references/assertions)에서 볼 수 있다.

#### 테스트 코드를 읽는 법

- `cy.get(".todo-list li")`는 쿼리 셀렉터 `.todo-list li`에 해당하는 태그(여럿)에 대해서라는 의미로 해석할 수 있다.
- `.should("have.length", 2)`는 대상의 갯수가 2개 라는 의미를 가진다. 여기서 대상은 앞서 태그를 선택했으므로 태그의 갯수가 된다.
- 곧, `.todo-list li`라는 쿼리 셀렉터에 해당하는 태그의 갯수가 2개인가?라는 의미를 가지고 있다.
- 페이지에서 `.todo-list li`으로 선택되는 태그가 1개 여도 안 되고, 3개여도 안 되고 딱 2개만 있어야 한다는 의미이다.

#### todo-list 태그의 첫 번째 li 태그가 'Pay electric bill'이란 값인지 확인하는 코드

- `cy.get(".todo-list li").first().should("have.text", "Pay electric bill");`
- 페이지에서 태그 선택자 `.todo-list li`에 해당하는 태그를 모두 선택하고 그 중 첫 번째 `.first()` 태그에 대하여, `.should("have.text"`로 태그는 텍스트를 가져야 하는데 그 값이 `"Pay electric bill"`이어야 테스트를 통과한다는 의미이다.
- `<li></li>` li 태그를 선택하고 그 안의 값을 문자열로 해석할 때 `<li>Pay electric bill</li>`와 같은 태그로 되어 있어야 테스트가 통과한다는 의미이다.

#### todo-list 태그의 마지막 li 태그가 'Walk the dog'이란 값인지 확인하는 코드

- `cy.get(".todo-list li").last().should("have.text", "Walk the dog")`
- `<li>Walk the dog</li>`인지 확인하는 코드이다.
