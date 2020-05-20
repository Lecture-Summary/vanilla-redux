## Redux toolkit

많은 Redux의 코드를 더 적은량의 코드로 쓰게 도와주는 것

[Redux toolkit 문서 바로가기](https://redux-toolkit.js.org/introduction/quick-start)

## 설치

```sh
yarn add @reduxjs/toolkit
```

위 코드 실행시 redux toolkit이 설치됨

## createAction

```js
const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      return [{ text: action.text, id: Date.now() }, ...state];
    case deleteToDo.type:
      return state.filter((toDo) => toDo.id !== action.id);
    default:
      return state;
  }
};
```

위의 코드를

```js
import { createAction } from "@reduxjs/toolkit";

const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      return [{ text: action.text, id: Date.now() }, ...state];
    case deleteToDo.type:
      return state.filter((toDo) => toDo.id !== action.id);
    default:
      return state;
  }
};
```

createAction을 사용해 더 짧게 사용

그리고 addToDo를 type과 text를 가졌었다.

그러나 `console.log()` 하면 text 라는 것은 없고 payload라는 것이 있다.

payload는 관행 같은 것이다. redux toolkit이 우리에게 제공하는 것이다.

액션에 보내고 싶은 정보는 payload에 보내진다.

```js(6)
const addToDo = createAction("ADD");
const deleteToDo = createAction("DELETE");

const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      return [{ text: action.payload, id: Date.now() }, ...state];
    case deleteToDo.type:
      return state.filter((toDo) => toDo.id !== action.payload);
    default:
      return state;
  }
};
```

6번째 줄의 action.text를 action.payload로 바꿔줘야한다.

payload의 값을 보고 싶으면 `console.log(action)`을 입력하면 된다.
