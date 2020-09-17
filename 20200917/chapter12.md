싱글 페이지 애플리케이션
---

### 리액트 라우터

- 사용자가 입력한 주소를 감지하는 역할
- 여러 기기(환경)에서 동작 가능하도록 5가지 라우터 컴포넌트가 존재
  - **BrowserRouter: 브라우저 주소 감지 - 가장 보편적**
  - HashRouter: 해시 주소(`http://localhost#login`)을 감지
  - MemoryRouter: 메모리에 저장된 주소로 이동
  - NativeRouter: 리액트 네이티브
  - StaticRouter: 프로퍼티로 전달된 주소를 사용하는 라우터

### BrowserRouter 예제

- `yarn add react-router-dom`

```javascript
import React, { useState } from 'react';
import { BrowserRouter, Route, Switch, Link, Redirect } from 'react-router-dom';

const IWant = () => <div>IWant component</div>;
const To = () => <div>To component</div>;
const Sleep = () => <div>Sleep component</div>;

const App = () => {
  const [isActiveRedirect, setIsActiveRedirect] = useState(false);

  return (
    <div className="App">
      <h1>App</h1>
      <button type="button" onClick={() => setIsActiveRedirect(true)}>goOtherPage</button>
      <BrowserRouter>
        <ul>
          <li>
            <Link to="/">IWant</Link>
          </li>
          <li>
            <Link to="/to">To</Link>
          </li>
          <li>
            <Link to="/sleep">Sleep</Link>
          </li>
        </ul>
        <Switch>
          <Route path="/" exact>
            {isActiveRedirect && <Redirect to="/sleep" /> : <IWant>}
          </Route>
          <Route path="/to" component={To} />
          <Route path="/sleep" component={Sleep} />
        </Switch>
      </BrowserRouter>
    </div>
  );
}

export default App;
```

- `<Route>` : 요청된 주소가 해당 Route의 path와 맞을 경우 해당 컴포넌트를 렌더링.
- `<Switch>` : 배치된 Route 컴포넌트 중 처음으로 조건을 만족한 하나의 컴포넌트만 렌더링.
- `<Link>` : 주소와 리액트 라우터의 정보만 변경. 새로고침 현상 방지.
- 주소가 `/` 인 경우에는 `exact` 프로퍼티를 추가해야 한다. path가 `/` 로 시작하는 다른 컴포넌트에 영향을 주지 않도록, 즉 사용자가 입력한 주소가 path와 정확하게 일치할 경우에만 컴포넌트가 출력됨을 보장.