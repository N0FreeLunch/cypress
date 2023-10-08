## 열결되는 동작 테스트하기

### 대상 테스트 파일

- cypress/e2e/1-getting-started/todo.cy.js

### 예제 코드

```js
it("can add new todo items", () => {
  // We'll store our item text in a variable so we can reuse it
  const newItem = "Feed the cat";

  // Let's get the input element and use the `type` command to
  // input our new list item. After typing the content of our item,
  // we need to type the enter key as well in order to submit the input.
  // This input has a data-test attribute so we'll use that to select the
  // element in accordance with best practices:
  // https://on.cypress.io/selecting-elements
  cy.get("[data-test=new-todo]").type(`${newItem}{enter}`);

  // Now that we've typed our new item, let's check that it actually was added to the list.
  // Since it's the newest item, it should exist as the last element in the list.
  // In addition, with the two default items, we should have a total of 3 elements in the list.
  // Since assertions yield the element that was asserted on,
  // we can chain both of these assertions together into a single statement.
  cy.get(".todo-list li")
    .should("have.length", 3)
    .last()
    .should("have.text", newItem);
});
```

#### title

- 새로운 아이템을 추가한다는 의미를 가지고 있다.

#### 태그에 값 입력하기

- `cy.get("[data-test=new-todo]")`에서 `[data-test=new-todo]`는 CSS 선택자에 해당한다. 속성이 `data-test`인 태그의 부여된 속성값이 `new-todo`인 태그를 선택한다는 의미이다. 곧 `<어떤태그 data-test="new-todo"></어떤태그>`와 같은 형태의 태그를 선택한다는 것.
- `.type(`${newItem}{enter}`)`는 선택한 태그에 인풋값을 넣는 코드이다. 함수는 보통 행위를 나타내므로 동사를 사용한 표현을 사용한다. 따라서 여기서 `type`은 유형을 의미하는 명사가 아니라 흔히 `typing`으로 알고 있는 동사 `type`인 '(타자기컴퓨터로) 타자 치다[입력하다]'의 의미로 정의된 것이다.
- 여기서 `{enter}`라는 문자열을 추가했는데 이는 엔터를 친다는 의미이다. 입력한 문자열 값은 `${newItem}` 부분에 해당한다. 곧, 글자를 입력하고 엔터를 치는 코드까지 포함이 된 것이다.
- 여기서는 `should`라는 키워드를 사용하지 않았다. `should`라는 키워드는 대상이 무엇이어야 한다는 확인이며 어떤 조건의 테스트를 통과해야하는지를 정하는 것이다. 하지만 여기서는 어떤 테스트를 통과하는 것이 아닌 태그에 값을 입력하는 조작에 해당한다.

#### 추가된 태그 확인하기

```js
cy.get(".todo-list li")
  .should("have.length", 3)
  .last()
  .should("have.text", newItem);
```

- `.get(".todo-list li")`으로 클래스가 `todo-list`인 태그의 `li` 태그를 모두 선택하고 그 갯수가 3인지 확인하는 코드 `.should("have.length", 3)`그리고 마지막으로 추가된 태그가 가진 텍스트가 `newItem`인지 확인하는 코드 `.should("have.text", newItem);`가 포함되어 있다.
- 앞의 테스트 코드로 입력이 되었고 텍스트가 추가 되었는지 확인하기 위한 코드가

#### 태스트 연결 동작 이해하기

- 이전 순서의 테스트 코드의 타이틀은 `displays two todo items by default`이었다. 태그의 class가 `todo-list`인 태그에 `li` 태그가 2개 들어 있는지 확인하고, 각각의 태그 안에 들어 있는 text 값이 테스트 코드에서 지정한 값과 일치하는지를 확인하는 코드였다.
- 이번에 테스트 할 내용은 새로운 아이템을 추가를 했을 때를 테스트 하는 것이다. 새로운 아이템이 늘어나면 `li` 태그의 수가 늘어나게 되고, 늘어난 `li` 태그 안에는 새로운 `text` 값이 추가가 될 것이다.
- 기존 2개의 li 태그에서 3개의 li 태그가 되었고 입력한 값이 추가되어 생성된 li 태그인지 확인하는 것이다. 곧, 앞선 2개의 태그를 확인하는 코드를 통과 했기 때문에 새로 추가되어 태그가 3개가 된 것을 테스트 하는 동작이 태그 하나가 추가된 동작을 확인하는 테스트가 될 수 있었다.
