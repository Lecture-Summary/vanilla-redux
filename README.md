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

이제 값을 수정할 때 사용하는것이 action이다.

action은 redux에서 function을 부를 때 쓰는 두번째 paremeter, 혹은 argument이다.

```js
const countModifier = (count = 0, action) => {
  console.log(action); // {type: "@@redux/INITp.b.n.o.c.r"} {type: "HELLO"}
  return count;
};

const countStore = createStore(countModifier);

countStore.dispatch({ type: "HELLO" });
```

reducer에 action을 보내는 방법이다.

dispatch에는 객체만이 들어갈 수 있고 반드시 type도 있어야한다.

type의 이름을 바꿀 수는 없다.

dispatch를 사용하면 redux가 countModifier를 호출할 것이고 console.log(action)이 호출되는 것이다.

```js
const countModifier = (count = 0, action) => {
  if (action.type === "ADD") {
    return count + 1;
  } else if (action.type === "MINUS") {
    return count - 1;
  } else {
    return count;
  }
};

const countStore = createStore(countModifier);

countStore.dispatch({ type: "ADD" });
countStore.dispatch({ type: "ADD" });
countStore.dispatch({ type: "ADD" });
countStore.dispatch({ type: "ADD" });
countStore.dispatch({ type: "ADD" });
countStore.dispatch({ type: "MINUS" });

console.log(countStore.getState()); // 4
```

dispatch로 type: "ADD" 를 5번 보냄으로써 count + 5가 되고

마지막에 type: "MINUS"를 1번 보냄으로써 count - 1이 되고 최종적으로 4가 찍히는 모습이다.

## subscribe

subscribe는 state안에 있는 변화들을 알 수 있게 해준다.

```js
const onChange = () => {
  console.log(countStore.getState());
};

countStore.subscribe(onChange);
```

위의 결과로 state가 변경되면 console.log 결과로 값이 변경되는 것을 볼 수 있다.

### 개선사항

if를 쓰는것보다 switch를 사용한다.

redux 공식문서에서도 switch를 사용하고 훨씬 낫다.

두번째 개선사항으로는 type: string을 사용하지않고 상수를 사용해주는 것이다.

그러면 "ADDD" 같은 오타가 발생했을때 string으로는 js 가 에러를 알려주지 않지만

type: ADD 로 할 경우 오타로 type: ADDD 로 했을경우 js 는 ADDD가 선언되지 않았다고 에러를 알려줄 것이다.

```js
const ADD = "ADD";
const MINUS = "MINUS";

const countModifier = (count = 0, action) => {
  switch (action.type) {
    case ADD:
      return count + 1;
    case MINUS:
      return count - 1;
    default:
      return count;
  }
};

const countStore = createStore(countModifier);

const onChange = () => {
  number.innerText = countStore.getState();
};

countStore.subscribe(onChange);

add.addEventListener("click", () => countStore.dispatch({ type: ADD }));
minus.addEventListener("click", () => countStore.dispatch({ type: MINUS }));
```

MUTATE STATE는 쓰면 안됨

store.getCurrent() + 1 같은 방법은 안되고 store를 수정하는 방법은 action을 보내는 것

`return state.push(action.text)` 와 같은 방법은 안된다.

새로운 배열을 만들어 return 한다.
