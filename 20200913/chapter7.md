리덕스로 데이터 관리하기
---

[1. 리덕스 기초 알아보기](#리덕스-기초-알아보기)

[2. 액션과 리듀서의 관계 알아보기](#액션과-리듀서의-관계-알아보기)

[3. 데이터를 위한 컴포넌트 알아보기](#데이터를-위한-컴포넌트-알아보기)

## 리덕스 기초 알아보기

- 리덕스: 스토어 중심 데이터 관리 라이브러리. 컨텍스트와 달리 단일 스토어에서 모든 데이터를 관리한다.

- 리덕스 VS 컨텍스트 (저자 의견)
  - 리덕스: 서버에서 가져온 데이터로 새로운 결과물을 만드는 경우
  - 컨텍스트: 컴포넌트의 통합 데이터를 관리할 경우

- 리덕스 동작 과정

  ![리덕스 동적 과정](https://miro.medium.com/max/1400/1*EdiFUfbTNmk_IxFDNqokqg.png)
  *출처 - [Integrating semantic-ui modal with Redux](https://itnext.io/integrating-semantic-ui-modal-with-redux-4df36abb755c)*

  - **dispatch**: 리듀서에 액션을 전달하는 함수
  - **action**: 액션에는 `동작 이름`, `데이터`가 객체로 들어감
  - **reducer**: 액션을 받아서 스토어에 변경 작업을 진행
  - **store**: 데이터 관리 공간
  - **connect**: 스토어 변경작업 후 연결된 컴포넌트에 변경된 데이터를 전달, 동기화 작업 진행하는 함수

- 리덕스 스토어 생성
  ```javascript
  import React from 'react';
  import { createStroe } from 'redux';
  import { Provider } from 'react-redux';
  import { composeWithDevTools } from 'redux-devtools-extension' // 크롬 개발자 도구

  import reducer from './reducers';
  import App from './App';

  const store = createStore(reducer, composeWithDevTools());
  const ReduxApp = () => (
    <Provider store={store}>
      <App />
    </Provider>
  );
  ```

## 액션과 리듀서의 관계 알아보기

- **Action**: `{ type: 'ACTION_NAME', payload: 'DATA'}`
  - type: 액션을 나타내는 고유 값. 'Type' 명칭은 변경 불가
  - payload: 스토어에 사용될 값. 명칭 변경 및 생략 가능

- **reducer**: 
  ```javascript
  const reducer = (state, action) => {
    switch (action.type) {
      CASE ACTION_TYPE_1:
        return { ...state, value: myNewState1 };
      CASE ACTION_TYPE_2:
        return { ...state, value: myNewState2 };
      default:
        return state;
    }
  }
  ```
  - 리듀서는 state, action을 받아 새로운 스토어의 데이터를 반환
  - 리듀서가 반환하는 값의 자료형은 스토어의 이전 데이터와 동일해야 함

---

P.S. 그 뒤로 책에 있는 내용이 너무 예전 내용이라서, [react redux 공홈](https://react-redux.js.org/introduction/basic-tutorial) 보고 내용 정리하겠습니다. ~~2020년 책인데...~~

```javascript
import { connect } from 'react-redux';

import { increment, decrement, reset } from "./myActions";

// state에서 필요한 데이터 가공 후 props로 사용 + ownProps는 옵션
const mapStateToProps = (state, ownProps) => {
  const { users } = state;
  const { id } = ownProps;

  return { user: users[id] };
};

// action creators를 담고있는 객체
// 함수형으로 작성 가능하지만, 공홈에서는 객체형으로 구현하는 것을 권장하고 있음
const mapDispatchToProps = {
  increment,
  decrement,
  reset
};

// `connect`는 컴포넌트와 스토어를 연결하는 함수
// function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)(Component)
connect(
  mapStateToProps,
  mapDispatchToProps
)(SampleComponent);
```

## 데이터를 위한 컴포넌트 알아보기

리액트의 장점은 컴포넌트를 쉽게 재구성하고 공유하기 편하다는 것. 그런데 리덕스를 포함하고 있는 컴포넌트를 다른 앱에서 재사용하기 위해서는 리덕스 스토어를 동일하게 구성해야 하는 문제가 존재. -> `화면 컴포넌트`와 `데이터 컴포넌트`를 구분해서 구현

- 화면 컴포넌트
  - 화면 출력용
  - 프로퍼티로 데이터를 전달받기만 함.
  - 데이터 변경은 프로퍼티로 전달받은 콜백 함수를 호출하거나 state 사용

- 데이터 컴포넌트
  - 스토어의 데이터를 컴포넌트에 전달 및 변경용
  - react-redux의 `Provider`를 통해 스토어와 연결, 데이터 관리.
  - reducer, dispatch 함수를 화면 컴포넌트에 프로퍼티로 전달
  - Consumer 컴포넌트 형태