## 에어비앤비 디자인 시스템 따라 하기

Tools

- [StoryBook](https://storybook.js.org)
- [SASS](https://sass-guidelin.es)
  - [SASS vs SCSS vs LESS](https://goohwang.tistory.com/5)
- [Material Design](https://materializecss.com): 구글에서 공개한 디자인 가이드
- node-sass: scss 파일을 컴파일하여 CSS 파일로 생성
- sass-loader: webpack 에 sass 추가
- [Styled-components](https://styled-components.com)
  - 컴포넌트 안에서 스타일을 정의해 바로 사용
  - 글로벌 스타일 시트를 더럽히지 않는 CSS-in-JS 방식
  - HOC 로 구성
  - 서버출력(;서버출력이란 화면 출력을 시작하는 순간에 스타일 코드를 서버에서 생성하여 같이 출력: HOC 구조로 컴포넌트의 동기화)이므로 깜빡임 현상이 없음
- Aphrodite: 서버 출력을 도와주는 라이브러리
- enzyme: jest 로 할 경우 리액트를 DOM 함수를 써야하는 불편함이 있는데 이를 편하게 해주는 테스트 도구

Media 속성 값

- small: mobile
- medium: table
- large: desktop
- largeAndAboves: 모니터에 특화된 해상도

```javascript
function withStyles(styleFunc) {
  // ...styleFunc code: https://github.com/airbnb/react-with-styles/blob/master/src/withStyles.jsx#L193
  // THIS CODE: https://github.com/airbnb/react-with-styles/blob/master/src/withStyles.jsx#L122
  return function getComponent(Component) {
    return class WithStyleComponent extends React.Component {
      render() {
        return <Component {...this.props} />;
      }
    };
  };
}

class Text extends React.PureComponent {
  render() {
    const { children, styles } = this.props;
    return <span {...css(styles.default)}>{children}</span>;
  }
}

export default withStyles(({ color, size }) => ({
  default: { color: color.default, fontSize: size.md },
}))(Text);
```
