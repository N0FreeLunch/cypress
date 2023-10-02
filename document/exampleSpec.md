## 예제 코드 생성하기

- (http://localhost:54150/\_\_/#/specs)와 같은 경로로 처음 사이프레스 관리화면에 접속하게 되면 실행할 수 있는 아무런 대상도 없다. '+ New spec'이란 버튼이 있는데 Scaffold example specs라는 버튼이 존재한다. 그럼 'cypress/e2e'라는 폴더에 예제 테스트 코드가 생성된다.
- 'Specs'라는 메뉴에서 'cypress' 폴더에 들어 있는 코드들을 실행해 볼 수 있다.

## 예제 코드 테스트 해 보기

- 브라우저의 사이프레스 관리화면에서 Specs 페이지의 cypress/e2e/1-getting-started/todo.cy.js 경로의 파일을 실행해 보자. 그럼 다음과 같은 테스트 항목이 나타나고 해당 사이프레스 코드에 정의된 테스트 코드에 따라 테스트를 수행하게 된다.
- 테스트 항목

  - example to-do app
  - displays two todo items by defaultpassed
  - can add new todo itemspassed
  - can check off an item as completedpassed
  - with a checked task
  - can filter for uncompleted taskspassed
  - can filter for completed taskspassed
  - can delete all completed tasks

- 해당 테스트 파일은 'https://example.cypress.io/todo'라는 페이지의 실행동작을 확인한다.
- 테스크가 실행되었다면 테스트 항목에 초록색의 체크 표시가 나타나게 된다.
