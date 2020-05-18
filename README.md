# React-Redux

```sh
yarn add react-redux
```

react에서 redux를 사용할때 react-redux를 redux와 함께 설치한다.

그 후 store.js 를 생성한다.

store.js 에는 js에서 redux를 사용하듯이 쓰면된다.

```js
import { createStore } from "redux";

const ADD = "ADD";
const DELETE = "DELETE";

export const addToDo = (text) => {
  return {
    type: ADD,
    text,
  };
};

export const deleteToDo = (id) => {
  return {
    type: DELETE,
    id,
  };
};

const reducer = (state = [], action) => {
  switch (action.type) {
    case ADD:
      return [{ text: action.text, id: Date.now() }, ...state];
    case DELETE:
      return state.filter((toDo) => toDo !== action.id);
    default:
      return state;
  }
};

const store = createStore(reducer);

export default store;
```

대신 다른곳에서 사용할 수 있게 export를 해주어야한다.

그 후 js에서는 subscribe를 해주었는데 react에서는 subscribe 부분을 react-redux에서 제공하는 기능을 사용한다.

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";
import { Provider } from "react-redux";
import store from "./store";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

index.js로 간 후 react-redux의 Provider를 import해준다.

그 후 `<App/>` Component를 `<Provider>`로 감싸준다.

Provider에는 store={} 값으로 export한 store를 넣어준다.
