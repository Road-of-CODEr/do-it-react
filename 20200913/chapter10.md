## 리덕스 고급 기능 활용하기

### index

1. 미들웨어 기초
2. redux-thunk 와 비동기 제어
3. 서버 지연 처리와 오류 표시하기
4. 미들웨어로 알림 메시지 띄우기
5. 코인 거래 알림 효과 추가하며 마무리하기

시작하기 전에 [이 글(;React + Redux 플로우의 이해)](https://medium.com/@ca3rot/%EC%95%84%EB%A7%88-%EC%9D%B4%EA%B2%8C-%EC%A0%9C%EC%9D%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%AC%EC%9A%B8%EA%B1%B8%EC%9A%94-react-redux-%ED%94%8C%EB%A1%9C%EC%9A%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-1585e911a0a6)을 읽으면 매우 도움 됨

> [리액트의 생성 과정이 궁금하면 여기 클릭](https://pomb.us/build-your-own-react)

미들웨어란?

- 리덕스의 검문소 역할을 함.
- 특정 액션에 연쇄로 일어나는 액션을 추가하거나 추가 작업을 할 수 있게 해줌.
- 미들웨어는 액션과 스토어 사이 리듀서들을 묶어 관찰한다.

```javascript
// 마지막 nextRunner 함수 전에서 리듀서 실행 이전 미들웨어,
// 함수 작동 후 에서 리듀서 실행 이후 미들웨어가 작동한다.
const middleware = (store) => (nextRunner) => (action) => nextRunner(action);

middleware(store)(nextRunner)(action);
```

사용해보기

```javascript
import { createStore, combineReducers, applyMiddleware } from "redux";

const customMiddleware = (store) => (nextRunner) => (action) => {
  console.log("미들웨어에 전달된 액션 객체", action);
  console.log("리듀서 실행이전", store.getState());
  const result = nextRunner(action);
  console.log("리듀서 실행이후", store.getState());
  return result;
};

export default (initStates) =>
  createStore(
    combineReducers(reducers),
    initStates,
    applyMiddleware(customMiddleware) // custom middleware
  );
```

### 다중 미들웨어

redux 에서 제공해주는 `applyMiddleware` 는 `compose()` 함수와 같은 형태로 구현되어 있어 다중 인자로 결합해 줄 수 있다.

```javascript
export default (initStates) =>
  createStore(
    combineReducers(reducers),
    initStates,
    applyMiddleware(customMiddleware, customMiddleware2, customMiddleware3)
  );
```

### redux-thunk 와 비동기 제어

![[thunk architecture](https://designingforscale.com/understanding-redux-middleware-and-writing-custom-ones)](https://designingforscale.com/content/images/2017/09/reduxMiddleware.png)

- 리덕스에서 데이터는 단방향으로 흘러야 한다.
- 제각각 완료되는 비동기 작업의 결과가 단방향의 액션 호출 순서로 진행되어 데이터의 흐름을 시간 순서대로 처리.
- 비동기 지연 작업을 처리하기 위해선 콜백 함수를 사용해야 한다.(액션 함수 내에서 dispatch() 함수 호출) 하지만 객체 형태의 액션은 콜백 함수 형태로 처리할 수 없다.
  - 이 문제를 `redux-thunk` 가 해결.
  - 액션이 객체가 아닌 함수 형태를 가질 수 있게 함으로써 비동기 처리를 손쉽게 구현.

`redux-thunk` 미들웨어의 구조

```javascript
const thunk = (store) => (nextRunner) => (action) => {
  if (typeof action === "function") {
    return action(store.dispatch, store.getState);
  }

  return nextRunner(action);
};
```

- 모든 `action` 을 감시해 함수일 경우 콜백 함수 형태로 액션을 호출함.
- `action` 이 객체가 아닌 함수형태다.(기존의 액션은 `type`, `payload`로 구성되어 있음.)
- **dispatch 함수와 스토어 데이터를 액션에 포함시킬 수 있다.**

```javascript
function thunkAction(somePayload) {
  return function (dispatch, getState) {
    dispatch({
      type: "SOME_ACTION_TYPE",
      payload: somePayload,
    });
    dispatch({
      type: "SOME_ACTION_TYPE2",
      payload: {
        ...getState().resource,
        somePayload,
      },
    });
  };
}
```

- 두 가지 `dispatch` 함수를 순서대로 호출.
- `getState().resource` 로 스토어 데이터의 객체를 참조할 수 있음.

### 서버 지연 처리와 오류 표시하기

- 서버의 데이터가 오고있다는 것을 알려주는 지연 처리 혹은 에러 페이지가 필요.

```javascript
export function requestTransactionList(params) {
  return dispatch => {
    // start loading
    dispatch(loading());
    Api.get(`/url`, ({params})).then(({data}) => {
      // end loading
      dispatch(setTransactionList(data);
    });
  }
}
```
