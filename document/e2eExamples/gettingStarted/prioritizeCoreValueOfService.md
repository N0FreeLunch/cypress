## 서비스 제공의 핵심 가치를 우선하여 테스트를 작성하자

- CSS로 화면이 만들어지는 부분에 대한 테스트는 태그 기반으로 확인할 수 있는 테스트보다 구성하기 어렵기 때문에 보통은 태그 기반으로 확인할 수 있는 부분까지 테스트를 작성한다.
- 물론 대상의 화면 상에서의 위치 및 크기 등의 모든 사양을 테스트 하고 싶을 경우도 있다. 이 경우에 대한 테스트를 cypress를 통해서 구성할 수 있지만, 태그 수준에서 확인할 수 있는 것 보다 테스트 코드를 작성하기 좀 더 까다로울 수 있다.
- 테스트 코드를 작성하는 것은 시간이 걸리는 일이기 때문에 필요하다고 판단되는 부분에만 작성해서 생산성을 높이는 것이 좋다. 따라서 CSS의 표시에 관한 부분은 사람의 눈대중에 맡기고, 사람이 간단하게 확인하기 어려운 CSS의 동작만 테스트를 구성하여 보조하여, 무조건 전체 화면의 디자인 사양을 만족하는지에 대한 테스트를 구성하기 보다는 테스트하지 않으면 서비스 경험에 치명적인 문제를 가져다 주는 부분에 대해서 주로 테스트를 작성하도록 한다.

### 예제 코드

```js
it("can check off an item as completed", () => {
  // In addition to using the `get` command to get an element by selector,
  // we can also use the `contains` command to get an element by its contents.
  // However, this will yield the <label>, which is lowest-level element that contains the text.
  // In order to check the item, we'll find the <input> element for this <label>
  // by traversing up the dom to the parent element. From there, we can `find`
  // the child checkbox <input> element and use the `check` command to check it.
  cy.contains("Pay electric bill")
    .parent()
    .find("input[type=checkbox]")
    .check();

  // Now that we've checked the button, we can go ahead and make sure
  // that the list element is now marked as completed.
  // Again we'll use `contains` to find the <label> element and then use the `parents` command
  // to traverse multiple levels up the dom until we find the corresponding <li> element.
  // Once we get that element, we can assert that it has the completed class.
  cy.contains("Pay electric bill")
    .parents("li")
    .should("have.class", "completed");
});
```

- 이번 테스트의 타이틀은 `"can check off an item as completed"`이다. `check off`는 체크 표시를 하다라는 동사로 li와 같은 항목(item)에 대해 체크를 하는 동작을 통해서 완료(`completed`)가 할 수 있는지를 확인하기 위한 테스트 코드임을 알 수 있다.

<!-- prettier-ignore -->
```js
cy.contains("Pay electric bill")
  .parent()
  .find("input[type=checkbox]")
  .check();
```

- `contains`라는 함수는 페이지의 태그 중에서 지정한 문자열을 포함하는 태그 선택한다. 문자열을 포함한 태그가 여럿이라면 여러 개를 선택한다.
- `contains("Pay electric bill")`라는 코드는 `Pay electric bill`라는 텍스트를 포함하는 태그를 선택한다.
- `.parent()`는 부모 태그를 선택한다는 의미이다. 만약 여러개의 태그가 `contains`에 의해 선택이 되어 있다면 각각의 태그의 부모 태그가 선택이 된다.
- 선택된 부모 태그의 하위에 있는 CSS 선택자 `input[type=checkbox]`에 해당하는 태그를 찾는다. `<input type="checkbox" />`에 해당하는 태그를 뽑는다.
- 만약 리스트 중에서 `.find` 함수에 의해 선택되는 대상이 없는 것이 있다면 여러 개의 태그를 중에서 선택되는 태그 리스트만 남겨진다.
- `<input type="checkbox" />`은 브라우저에서 체크박스에 해당한다. `.check()` 함수는 체크박스에서 체크를 선택하는 기능이다.
- 곧, `Pay electric bill`이란 문자열이 들어 있는 태그에서 그 부모 태그의 하위 태그의 체크박스에 체크를 선택한다는 의미를 가지고 있다.

```js
cy.contains("Pay electric bill")
  .parents("li")
  .should("have.class", "completed");
```

- 동일한 바로 앞에서 체크 버튼을 누르는 코드 동작과 동일한 항목 `"Pay electric bill"`을 선택하였다.
- `contains("Pay electric bill")`로 선택된 태그에서 `.parents("li")`으로 부모 태그가 `li`인 태그를 선택한다는 의미이다.
- `.should("have.class", "completed")` 부분은 해당 `li` 태그는 `completed`라는 클래스가 지정되어 있어야 한다. 만약 여러 태그가 대상이 되었을 때는 모두 클래스가 `completed`으로 되어야 테스트를 통과한다.

### 동작의 흐름 이해하기

- 위의 코드와 실제 브라우저의 동작을 보고 어떤 동작을 테스트 하는 상황인지 이해, 추측을 통해 상황을 구성할 수 있어야 한다.
- todo 리스트의 항목들 중에서 지정한 타이틀을 포함하는 항목이 체크가 되는지를 확인하기 위한 테스트 코드를 구성하였다는 것을 이해해야 한다.
- 체크 버튼을 누르면 태그의 클래스가 `completed`으로 바뀌면서 해당 클래스에 부여된 CSS를 통해서 체크 버튼이 생성되는 동작이라는 것을 확인할 수 있다.
- 브라우저에서의 동작을 살펴 보면 어차피 체크 표시는 CSS으로 만들어지기 때문에 태그 기반으로 동작을 확인하는 cypress으로 화면의 표시에 대한 테스트를 하기 어려우므로 CSS를 적용하기 위한 태그의 클래스 변경이 이뤄지는 것까지만을 테스트하기로 한다.
- 전체 과정은 체크 버튼을 누르고 체크 버튼을 선택함하여 항목의 `li` 태그에 `completed` 클래스가 표기되느냐를 테스트한다.
