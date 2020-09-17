## 에어비앤비 개발 방식으로 비동기 제어하기

`Promise` 클래스의 대략적인 구조

```javascript
class Promise {
  constructor() {
    // 1. start
  }
  then(...) {
    // 2. success
  }
  catch(...) {
    // 3. failure
  }
  finally(...) {
    // 4. finish
  }
}
```

`redux-pack` 은 `Promise` 의 각 과정을 감지하여 액션을 호출하도록 구현되어 있다.
(액션을 자동으로 호출하므로 개발자가 별도의 액션을 구성하지 않아도 된다는 뜻.)

NONE
