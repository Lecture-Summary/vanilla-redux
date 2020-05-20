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

createReducer를 import한다.

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

위 코드는 createReducer를 사용하기 전이다.

```js
const reducer = createReducer([], {
  [addToDo]: (state, action) => {
    state.push({ text: action.payload, id: Date.now() });
  },
  [deleteToDo]: (state, action) =>
    state.filter((toDo) => toDo.id !== action.payload),
});
```

위 코드는 createReducer를 사용한 후의 코드이다.

createReducer의 첫번째 인자에는 initial state를 넣고, 두번째 인자는 action이라고 생각하면 된다.

createReducer를 사용하면 state를 mutate하기 쉽게 만들어준다.

createReducer를 사용할땐 두가지 옵션이 있다.

원래 하던대로 새로운 state를 return 할 수 있고, 수정한 작업처럼 state를 mutate할 수 있다.

mutate가 가능한 이유는 immer 아래에서 작동이 되기 때문에 redux toolkit이 정보를 가져가 암튼 된다.

그래서 mutate를 하는데 뒤에선 return하여 새로운 state 생성 기능이 수행된다.

무언가를 return할때는 filter처럼 새로운 state여야 한다.

state를 mutate하고 싶다면 return 하지 않아야한다.

위의 코드의 push는 배열을 mutate를 해서 return을 하지 않는 모습이고, filter는 mutate를 하지 않고 새로운 array를 생성해 return 하는 모습이다.

## configureStore

```js
import { createAction, createReducer, configureStore } from "@reduxjs/toolkit";
```

configureStore를 import 한 후

```js
const store = createStore(reducer);
```

기존의 createStore를

```js
const store = configureStore({ reducer });
```

아래와 같이 변경하면 구글 크롬의 extension Redux devtools의 불이 들어온다.

redux devtools는 우리의 state를 볼 수 있게 해준다.

어떤 action이 발생했고 언제 발생했는지도 알려준다.

그리고 Redux devtools의 dispatcher를 사용해 dispatch를 보내 볼 수도 있다.

Redux devtools를 사용하기 위해 redux toolkit이 반드시 필요한건 아니다. 하지만 자동으로 실행시켜준다.

[이미지]

[이미지2]

[이미지3]
