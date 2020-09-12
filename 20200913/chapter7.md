리덕스로 데이터 관리하기
---

[1. 리덕스 기초 알아보기](#리덕스-기초-알아보기)

[2. 액션과 리듀서의 관계 알아보기](#액션과-리듀서의-관계-알아보기)

[3. 그래프 데이터베이스 도입하기](#그래프-데이터베이스-도입하기)

[4. 데이터를 위한 컴포넌트 알아보기](#데이터를-위한-컴포넌트-알아보기)

[5. 검색 기능 만들면서 리덕스 복습하기](#검색-기능-만들면서-리덕스-복습하기)

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

