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

[image1]

## createReducer

```js
import { createAction, createReducer } from "@reduxjs/toolkit";
```

```js
const reducer = (state = [], action) => {
  switch (action.type) {
    case addToDo.type:
      return [{ text: action.payload, id: Date.now() }, ...state];
    case deleteToDo.type:
      return state.filter((toDo) => toDo.id !== action.payload);
    default:
      return state;
  }
```

```js
const reducer = createReducer([], {
  [addToDo]: (state, action) => {
    state.push({ text: action.payload, id: Date.now() });
  },
  [deleteToDo]: (state, action) =>
    state.filter((toDo) => toDo.id !== action.payload),
});
```

createReducer를 import한다.

createReducer를 사용하면 state를 mutate하기 쉽게 만들어준다.

createReducer의 첫번째 인자에는 initial state를 넣고, 두번째 인자는 action이라고 생각하면 된다.

state를 return 할 필요도 없다.

createReducer를 사용할땐 두가지 옵션이 있다. 원래 하던대로 새로운 state를 return 할 수 있고, 수정한 작업처럼 state를 mutate할 수 있다.

mutate를 하는 이유는 immer 아래에서 작동이 되기 때문에 redux toolkit이 정보를 가져가 암튼 된다

mutate를 하는데 뒤에선 return 새로운 state기능이 수행됨.

무언가를 return할때는 새로운 state여야 한다.

state를 mutate하고 싶다면 return 하지 않는다.

위는 push는 mutate를 해서 return을 하지 않는 모습이고, filter는 mutate를 하지 않고 새로운 array를 생성해 return 하는 모습

[image2]
