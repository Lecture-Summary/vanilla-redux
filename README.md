# Vanilla Redux

Leaning Vanilla-Redux and React-Redux

## Redux

### install

```sh
$ yarn add redux
```

위 명령어로 redux 사용이 가능하다.

```sh
$ npm install redux
```

npm 을 사용한다면 위 명령어를 사용할 수 있다.

### usage

```js
import { createStore } from "redux";

const reducer = () => {};

const store = createStore(reducer);
```

redux에는 createStore라는 함수가 있다.

store가 하는 일은 기본적으로 data를 넣을 수 있는 장소를 생성한다.

Store는 state 를 넣는 곳이다.

state는 어플리케이션에서 바뀌는 데이터를 의미한다.

createStore를 할때에는 reducer가 꼭 필요하다.

reducer는 함수이고 data를 modify한다.

reducer가 "hello"를 return한다면 "hello"가 어플리케이션에 있는 data가 될 것이다.

```js
const countModifier = () => {
  return "hello";
};

const countStore = createStore(countModifier);

console.log(countStore.getState()); // console : hello
```

createStore 함수로 만들어지는 store의 변수명과 reducer의 이름은 다른 것이어도 상관없다.

위 처럼 return하는 값이 application의 data가 된다.
