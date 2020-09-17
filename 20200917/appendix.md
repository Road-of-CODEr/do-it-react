## 코드 스플리팅으로 `bundle.js` 크기 줄이기

```javascript
class AsyncLoginPage extends PureComponent {
  componentDidMount() {
    import("./LoginPage").then(({ default: Component }) => {
      this.Component = Component;
      this.forceUpdate();
    });
  }

  render() {
    const Component = this.Component;
    return Component ? <Component {...this.props} /> : null;
  }
}
```

지연 로딩으로 구현하기

- 해당 컴포넌트에 접근하기 전까지 컴포넌트를 임포트하지 않으므로 줄일 수 있다.

## Firebase

- https://firebase.google.com/products/firestore

## SSR 도입하기

- `next.js`

## SSR 로 구동되는 서비스 배포하기

- `netlify`, `S3`
