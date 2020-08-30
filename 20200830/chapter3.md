리액트 컴포넌트

---

## 목차

[1. 컴포넌트를 표현하는 JSX](#컴포넌트를-표현하는-JSX)

[2. 컴포넌트와 구성 요소](#컴포넌트와-구성-요소)

[3. 컴포넌트에 데이터를 전달하는 프로퍼티](#컴포넌트에-데이터를-전달하는-프로퍼티)

[4. 컴포넌트 상태 관리하기](#컴포넌트-상태-관리하기)

[5. 컴포넌트의 생명주기](#컴포넌트의-생명주기)

[6. 클래스형 컴포넌트](#클래스형-컴포넌트)

[7. 함수형 컴포넌트](#함수형-컴포넌트)

[8. 배열 컴포넌트](#배열-컴포넌트)

[9. 컴포넌트에서 콜백 함수와 이벤트 처리하기](컴포넌트에서-콜백-함수와-이벤트-처리하기)

[10. Input 컴포넌트 만들면서 복습하기](#Input-컴포넌트-만들면서-복습하기)

## 컴포넌트를 표현하는 JSX

```javascript
import React from 'react';
import Sample from './Sample';

class App extends React.Component {
  render() {
    return (
      // JSX
      <div>
      	<p>Hey, this is competition</p>
      	<p>Ok?<p>
      	<Sample />
      </div>
    )
  }
}
```

- JS와 HTML을 동시에 작성할 수 있는 문법
- 추가 자료 : [React - Introducing jsx](https://ko.reactjs.org/docs/introducing-jsx.html)

<details>
  <summary style="cursor: pointer;">QUIZ: 리액트에서 JSX로 표현한 결과물은 어떻게 나오는지?</summary>
  <div markdown="1">
    리액트 엔진은 Babel을 이용해서 JSX를 `React.createElement()` 함수 호출로 JS 객체를 만든다. 이런 객체를 'React 엘리먼트'라고 표현하고, React는 이 객체를 읽어서 DOM을 구성한다. 따라서 JSX에 표현된 내용들은 JS 객체로 인식되기 때문에 `if`나 `for loop` 뿐만 아니라 인자나 함수의 반환값으로도 사용 가능하다.
  </div>
</details>



## 컴포넌트와 구성 요소

```javascript
// List.jsx
import React from 'react';
import Item from './item';

// props: {items - 리스트에 표시될 항목들을 담고 있는 Array}
const List = ({items}) => (
  <div className="list">
    items.map(item => <Item item={item} key={item.id} />)
  <div/>
);

export default List;
```

```javascript
// Item.jsx
import React from 'react';

const Item = ({item}) => (
  <div className="item">
    <p>{item.title}</p>
    <p>{item.contents}<p>
  </div>
);

export default Item;
```

- 컴포넌트 - `View` 영역을 독립적이고 재사용 가능하게 만드는 개념

<details>
  <summary style="cursor: pointer">QUIZ: 컴포넌트 구성 요소 세 가지?</summary>
  <div markdown="1">
    - 프로퍼티 : 상위 컴포넌트에서 하위 컴포넌트로 전달하는 데이터. 수정이 불가능하다.
    - state : 컴포넌트의 상태값을 저장하고 변경할 수 있는 데이터. 하위 컴포넌트로도 전달이 가능하다.
    - 컨텍스트 : 부모 컴포넌트에서 생성하고, 포함된 모든 자식 컴포넌트에서 사용할 수 있는 데이터.
  </div>
</details>



## 컴포넌트에 데이터를 전달하는 프로퍼티

```javascript
// List.jsx
import React from 'react';
import PropTypes from 'prop-types';

import Item from './item';

// props: {items - 리스트에 표시될 항목들을 담고 있는 Array}
const List = ({items}) => (
  <div className="list">
    items.map(item => 
      <Item
        key={item.id}
        title="myItem"            // String
        contents={item.contents}  // [String]
        id={item.id}              // String
        isDisplayId               // Boolean - 프로퍼티 명시 안할 경우 false, 할 경우 true
      >
        <div>This is children node</div>  // children
      </Item>
    )
  <div/>
);

List.propTypes = {
  items: PropTypes.array
};

export default List;
```

```javascript
// Item.jsx
import React from 'react';
import PropTypes from 'prop-types';

const Item = ({title, contents, id, isDisplay, children}) => (
  <div className="item">
    <p>{title}</p>
    <p>{contents[0]}<p>
    <p>{contents[1]}<p>  
    {isDiaply && <p>{id}</p>}
  </div>
);

Item.propTypes = {
  title: PropTypes.string,
  contents: PropTypes.array,
  id: PropTypes.string.isRequired,  // 필수 프로퍼티 지정 - isRequired
  idDisplay: PropTypes.bool,
  children: PropTypes.node
};

export default Item;
```

- 프로퍼티는 부모에서 자식으로 향하는 단방향 데이터이다.
- 수정이 불가능하다.
- Boolean 프로퍼티는 이름만 선언해도 전달 가능하다. 선언할 경우 true, 생략한 경우 false 값이 전달된다.
- Prop-types로 프로퍼티의 자료형을 선언할 수 있다. 
- 추가 자료 - [React - Type checking with proptypes](https://ko.reactjs.org/docs/typechecking-with-proptypes.html)

<details>
  <summary style="cursor: pointer">QUIZ: 프로퍼티의 default 값을 설정하는 방법?</summary>
  <div markdown="1">
    ```javascript
      import React from 'react';
      import PropTypes from 'prop-types';
    
      const Item = ({value: 123, text}) => (...);
    
      Item.defaultProps = {
      	text: 'hello',
      };
    ```
    프로퍼티의 default 값을 설정하는 방법은 두 가지이다. 프로퍼티를 선언하는 부분에서 값을 바로 넣는 방법과, prop-types를 이용해서 값을 선언하는 방법이 있다.
  </div>
</details>



## 컴포넌트 상태 관리하기

값을 저장하거나 변경할 수 있는 객체가 필요한 경우에는 `state` 를 사용한다.

```javascript
import React, {setState} from 'react';

const Counter = () => {
  // 상황에 따라 값이 변할 수 있는 값 - state
  // setState의 반환 값을 배열이며, [state(상태값), setState(상태변경함수)] 로 구성된다.
  // setState는 비동기로 동작한다.
  const [count, setCount] = setState(0);
  const handlePlus = () => setCount(count => count+1);  // setState안 함수의 인자는 이전 state값
  const handleMinus = () => setCount(count => count-1);	// 함수 반환값은 변경하고자 하는 state값
  const reset = () => setCount(0);
  
  return (
    <div>
      <div>{count}</div>
      <button onClick={handlePlus}>+</button>
      <button onClick={handleMinus}>-</button>
      <button onClick={reset}>reset</button>
    </div>
  )
};
```

- 추가 자료 - [React - State and Lifecycle](https://ko.reactjs.org/docs/state-and-lifecycle.html)

<details>
  <summary style="cursor: pointer">QUIZ: setState로만 state값을 변경할 수 있는 이유는?</summary>
  <div markdown="1">
    - setState로 값을 변경해야 컴포넌트를 다시 렌더링할 수 있음
    - 무결성을 유지하기 위해서
    - 객체일 경우 반드시 객체 단위로 update가 되도록 - react 렌더링 성능을 위해
  </div>
</details>



## 컴포넌트의 생명주기

컴포넌트의 lifecycle과 관련된 메서드들은 **클래스형 컴포넌트**를 작성할 때만 사용됨을 주의.

- 컴포넌트 생성
  1. constructor : 맨 처음 한 번만 호출. super() 함수를 가장 위에 호출해야 함.
  2. getDerivedStateFromProp : static 함수. this로 props, state에 접근 불가능. 인자로 전달된 값만 사용 가능. 주로 상위 컴포넌트에서 전달받은 프로퍼티로 state값을 연동할 때 사용되며, 반환값으로 state를 변경한다.
  3. render
  4. componentDidMount : 컴포넌트가 화면에 모두 표현된 이후 해야하는 작업을 여기서 진행.
- 컴포넌트 생성 완료
  1. getDerivedStateFromProp, shouldComponentUpdate : `shouldComponentUpdate` 는 프로퍼티 혹은 state값이 변경되었을 때 '화면을 새로 출력해야 하는지' 판단. 데이터 변화를 비교하는 작업과 화면 출력여부 판단에 대한 리소스가 필요하므로 리액트 성능에 많은 영향을 준다.
  2. render
  3. getSnapshotBeforeUpdate : 컴포넌트의 변경된 사항이 가상 DOM에 완성된 이후 호출됨. DOM 정보에 접근할 때 사용.
  4. componentDidUpdate : 실제 화면에 컴포넌트가 출력된 이후 호출됨. DOM 정보를 변경할 때 사용.
- 컴포넌트 갱신완료
  1. componentWillUnmount : 컴포넌트가 소멸되기 직전에 호출됨. 주로 event listener나 setInterval 등을 해제하는 작업을 여기서 진행.
- 컴포넌트 소멸 완료

<details>
  <summary style="cursor: pointer">QUIZ: 자식 컴포넌트에서 props를 state 초기값으로 사용할 때 부모 컴포넌트와 동기화 하는 방법?</summary>
  <div markdown="1">
    getDerivedStateFromProps(state, props) 메서드를 이용해서 현재 가지고 있는 state값과 props로 넘어온 값이 동일한지 비교 후 상태 값을 갱신한다.
  </div>
</details>



## 클래스형 컴포넌트

- 종류: Component, PureComponent
- Component 클래스 
  - 프로퍼티, state, 생명주기 함수가 들어있는 구조
- PureComponent 클래스
  - Component 클래스를 상속받음
  -  `sholudComponentUpdate()` 를 '얕은 비교' 하도록 재정의
  -  Component 클래스는 항상 render()를 호출하는 반면, PureComponent는 '얕은 비교를 통해 데이터가 변경되었을 경우'에만 render()를 호출

**질문** : Page 132에 결과에서 PureComponent가 한 번만 호출되는 이유는?



## 함수형 컴포넌트

- 함수형 컴포넌트는 state가 없는 함수형 컴포넌트(SFC, Stateless Functional Component)이다.
- 데이터(프로퍼티, 컨텍스트)를 입력으로 받아서 결과값(JSX)을 출력한다.
- Lifecycle 메서드를 사용할 수 없다.

<details>
  <summary style="cursor: pointer">QUIZ: class component vs functional component</summary>
  <div markdown="1">
    class component에서는 `this`를 사용해서 state, props 등에 접근 가능할 뿐만 아니라 lifecycle 메서드를 사용해서 동작을 구현한다. 또한 최종 출력물은 render()에서 반환한다.
    
    반면 functional component에서는 props, context를 파라미터로 받고, 출력하고자 하는 내용을 return 한다.
  </div>
</details>



## 배열 컴포넌트

- 배열 컴포넌트라는 정식 명칭은 없음. (저자가 임의로 명명한 카테고리)
- 배열에 여러 자료형을 넣을 수 있음을 이용해서 결과값 출력하는 방법
  - JSX, 컴포넌트, 혹은 데이터가 담긴 Array에서 map을 사용

<details>
  <summary style="cursor: pointer">QUIZ: key값으로 index를 지양해야하는 이유?</summary>
  <div markdown="1">
    key값은 고유한 값이 사용되어야 한다. 그런데 항목의 순서가 바뀔 수 있는 경우에는 index값이 원하는 데이터의 key값과 달라져서 이상한 데이터가 출력될 수 있다. [참고 - index as a key is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)
  </div>
</details>



## 컴포넌트에서 콜백 함수와 이벤트 처리하기



## Input 컴포넌트 만들면서 복습하기

- 프로퍼티로 값을 넘길 때 다양한 자료형이 사용될 수 있도록 중괄호(`{}`)로 감싼다.
- 프로퍼티 타입을 선언할 때 `oneOf([elem1, elem2, elem2..])` 을 통해서 사용 가능한 자료형을 선언할 수 있다.