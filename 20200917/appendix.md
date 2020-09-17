## 코드 스플리팅으로 `bundle.js` 크기 줄이기

- 지연 로딩으로 구현하기

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
