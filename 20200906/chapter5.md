# HOC

#### _keyword: [currying](https://velog.io/@kmp1007s/함수형-프로그래밍의-Currying), [decorator pattern](https://gdtbgl93.tistory.com/9), [HOC](https://ko.reactjs.org/docs/higher-order-components.html)_



## 커링

```javascript
const add = (x, y) => x + y;
const addTwo = x => add(x, 2);
const addThree = x => add(x, 3);

const addY = x => y => add(x, y);
const addFour = addY(4);
const addFive = addY(5);

const result1 = addFour(10); // 10 + 4 = 14
const result2 = addFive(123); // 123 + 5 = 128
const result3 = addY(7)(5); // 5 + 7 = 12

// => ((x + a) * b) - c
const equation = (a, b, c) => x => ((x + a) * b) - c;
const fomula = equation(2, 5, 7); // ((x + 2) * 5) - 7
const result4 = fomula(5); // ((5 + 2) * 5) - 7
```

- **커링** : 함수의 인자를 다시 구성, 필요한 함수를 만드는 패턴
  - 커링을 사용하는 입장에서는 보통 '인자를 하나씩 받도록' 구현하는게 일반적
  - 커링의 깊이가 너무 깊어지면 오히려 성능, 가독성이 안좋아짐



## HOC(Higher-Order Components) 기초

```javascript
// Basic HoC
export default function withHoC(Component) {
  return function WithHoC(props) {
    return <Component {...props} />;
  } 
}

// with Style HoC
export default function withStyles(styleFunc) {
  return function getComponent(Component) {
    return function WithStyleComponent(props) {
      return <Component {...props} />;
    }
  }
})
```

- **상속 패턴**
  - 다중 상속이 불가능하기 때문에 최상단으로부터 직렬로 상속을 받을 수 밖에 없음
  - 상속해야할 부모가 많아질 수록 상속의 depth가 깊어짐
  - 원치않은 상속을 받아야하는 경우가 생김
  - 중간에 추가적으로 상속받게되는 경우에 다른 컴포넌트에도 영향을 줌
- **데코레이터 패턴(Decorator pattern)**
  - 클래스 사이에 묶이지 않고 기능만을 공유하는 패턴
  - 필요한 기능만 가져와서 독립된 객체 생성 가능
  - 상속 구조와 상관없이 상위 클래스 또한 기능을 따로 탑재해서 객체 생성 가능
  - _Java_: Interface, _Javascript_: Currying
- **HOC**
  - HOC라는 명칭은 JS의 고차 함수(Higher-order function)에서 유래 - ex) 커링
  - 기존 컴포넌트에 기능을 추가한 새로운 컴포넌트를 반환하는 함수를 의미 - **확장 컴포넌트**
  - 확장 컴포넌트이므로 기존 컴포넌트에 필요한 모든 프로퍼티를 전달해야 함
  - 코드 재사용, 비스니스로직 은닉에 용이
  - 일반적으로 HOC와 확장 컴포넌트에 `with` prefix를 사용함



## HOC 라이브러리 - recompose

- branch() - 조건식에 따라 다른 HOC를 출력하는데 사용되는 함수

  - ```javascript
    branch(
      condition: props => boolean,
      left: HigherOrderComponent,
      [right: HigherOrderComponent]
    )(WrappedComponent)
    ```

  - condition이 참이면 left, 거짓이면 right 컴포넌트가 출력됨. right는 생략 가능하며, 생략할 경우 WrappedComponent를 그대로 출력

  - 예제

    ```javascript
    import React from 'react';
    import branch from 'recompose/branch';
    import Modal from './Modal';
    
    const isPopup = props => props.isPopup;
    const PopupButton = props => <Button disabled>This is Modal Window</Button>;
    
    export default branch(
      isPopup,
      () => PopupButton,
    )(Modal);
    
    // More Simple
    export default branch(
      ({ isPopup }) => isPopup,
      () => () => <Button disabled>This is Modal Window</Button>,
    )(Modal);
    ```

- compose() - 다중 HOC를 입력으로 들어온 순서대로 구성해서 반환하는 함수

  - 예제

    ```javascript
    import compose from 'recompose/compose';
    import withState from 'recompose/withState';
    import withPopup from './withPopup';
    
    const withPopupData = withState('isPopup', 'setIsPopup', false);
    
    const Component = () => <span>팝업 완료</span>;
    const WithMsg = withMsg('팝업!')(Component);
    const WithPopup = withPopupMsg(WithMsg);
    
    export default withPopupAndMsg = compose(withPopupData, withPopup('팝업 중'));
    ```

    