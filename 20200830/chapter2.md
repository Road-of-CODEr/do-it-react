리액트 ES6 문법 액기스
---

## 목차

[1. 템플릿 문자열](#템플릿-문자열)

[2. 펼침 연산자](#펼침-연산자spread-operator)

[3. 가변 변수와 불변 변수](#가변-변수와-불변-변수)

[4. 클래스](#클래스)

[5. 화살표 함수](#화살표-함수)

[6. 객체 확장 표현식과 구조 분해 할당](#객체-확장-표현식과-구조-분해-할당)

[7. 라이브러리 의존성 관리](#라이브러리-의존성-관리)

[8. 배열 함수](#배열-함수)

[9. 비동기 함수](#비동기-함수)

[10. 디바운스와 스로틀](#디바운스와-스로틀)

## 템플릿 문자열

```javascript
const a = 2;
console.log('h' + a + 'i');
// Template Literal === ``
// Expression Interpolation === ${}
console.log(`h${a}i`); 

// Tagged Templates: 템플릿 리터럴로 감싸진 내용들을 인자로 하여 함수에 대입.
//      첫 번째 인자: Expression Interpolation 을 기준으로 문자열들이 분할되어 배열로 전달된다.
//      이후의 인자들: Expression Interpolation 의 값들이 전달됨.
const name = '레쉬';
const age = 22;

function flip(strings, personName, personAge) {
  const str0 = strings[0]; // Hello, 
  const str1 = strings[1]; // , you are 
  return personAge + str1 + personName + str0;
}

const input = `Hello, ${name}, you are ${age}`;
const output = flip`Hello, ${name}, you are ${age}`;

console.log(input); // Hello, 레쉬, you are 22
console.log(output); // 22, you are 레쉬Hello, 

// Raw strings
function tag(strings) {
  //  ↵  와 \n 의 차이!
  console.log(strings); // ['string text line 1 ↵ string text line 2', raw: ['string text line 1 \n string text line 2']];
  console.log(strings.raw[0]); // 'string text line 1 \n string text line 2'
}

tag`string text line 1 \n string text line 2`;
// 
```

> 추가 자료: [템플릿 리터럴(Template Literal)과 스타일드 컴포넌트](https://velog.io/@lesh/%ED%85%9C%ED%94%8C%EB%A6%BF-%EB%A6%AC%ED%84%B0%EB%9F%B4Template-Literal%EA%B3%BC-%EC%8A%A4%ED%83%80%EC%9D%BC%EB%93%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8styled-component)

<details>
<summary style="cursor: pointer;">QUIZ: 템플릿 리터럴 vs 표현식 삽입법 vs 태그된 템플릿 vs Raw string</summary>
<div markdown="1">

위의 코드 참고.

</div>
</details>

## 펼침 연산자(Spread operator)

```javascript
const arr = [ 1, 2, 3, 4, 5 ];
console.log([arr[0], ...arr]); // [ 1, 1, 2, 3, 4, 5 ];

const [first, ...rest] = arr;
console.log(first, rest); // 1, [ 2, 3, 4, 5, ];

const obj = {
  a: 1,
  b: 2,
  c: 3
}
const obj2 = {
  d: 4
}
console.log({...obj, ...obj2}); // { a: 1, b: 2, c: 3, d: 4};
```

<details>
<summary style="cursor: pointer;">QUIZ: 두 객체를 병합할 때 중복된 키값들은 어떻게 되는가?</summary>
<div markdown="1">

뒤의 값이 앞의 값을 덮어쓴다.

</div>
</details>

## 가변 변수와 불변 변수

- let vs const

<details>
<summary style="cursor: pointer;">QUIZ: Immutable 에 대해 let 과 const 를 엮어 설명해 보시오.</summary>
<div markdown="1">

let 은 가변이며 const 는 불변이다.

이때의 "불변"은 주소 값에 대한 불변이므로 해당 주소 안의 데이터(값)은 변경이 된다.

따라서 `const arr = [];` 와 같이 const 로 선언된 배열에 push 등으로 값을 변경할 수 있다.

Immutable 은 "불변"으로 주로 `Object.freeze` 와 같이 더이상 값 또한 변할 수 없는 상태를 뜻한다.(freeze 또한 완벽한 불변을 줄 순 없다)

</div>
</details>

<details>
<summary style="cursor: pointer;">QUIZ: 두 객체를 병합할 때 중복된 키값들은 어떻게 되는가?</summary>
<div markdown="1">

뒤의 값이 앞의 값을 덮어쓴다.

</div>
</details>

<details>
<summary style="cursor: pointer;">QUIZ3: 가변 내장 함수와 무결성 내장 함수를 선택하시오</summary>
<div markdown="1">

| 가변 함수 | 무결성 함수 |
| --------- | ----------- |
| push      | concat      |
| splice    | slice       |
| pop       | slice       |

</div>
</details>

## 클래스

기존 자바스크립트 문법에는 클래스 표현식이 없어 `prototype` 으로 클래스를 표현했다.

```javascript
function Shape(x, y) {
  this.name = 'Shape';
  this.move(x, y);
}
Shape.create = function(x, y) { return new Shape(x, y); }; // static method
Shape.prototype.move = function(x, y) { // instance method
  this.x = x;
  this.y = y;
}

class Shape {
    static create(x, y) { return new Shape(x, y); }
    name = 'Shape';
    constructor (x, y) {
        this.move(x, y);
    }
    move(x, y) {
        this.x = x;
        this.y = y;
    }
    area() {
        return 0;
    }
}
const a = Shape.create(1, 2); // OK.
a.create(2, 3); // ERROR! -> a.create is not a function.
```

`prototype` 은 객체를 생성함에 있어서 같은 메모리를 공유하므로 메모리 자원을 절약할 수 있다.

`prototype` 객체는 `new` 연산자로 생성되는 객체 안에서 this 연산자의 함수 및 변수 선언 위치를 참조할 수 있다.

> [정적 메서드와 정적 프로퍼티](https://ko.javascript.info/static-properties-methods)

<details>
<summary style="cursor: pointer;">QUIZ: static method 의 장단점을 설명해 보시오</summary>
<div markdown="1">

정적 메소드는 "인스턴스 없이 호출 가능" 하며 "인스턴스 객체에서는 호출할 수 없다". utility 함수에 적합(Math.max)

단점으로는 Global 하게 어디서든 접근할 수 있어 "상태"를 가지게 될 경우 치명적인 버그로 이어질 수 있다.

</div>
</details>

## 화살표 함수

```javascript
function a(a) { // function a : 이름이 a 인 함수
  this.a = a;
  console.log(a, this.a);
};

const a2 = (a) => { // () => {}: 익명함수
  this.a = a;
  console.log(a, this.a);
};

class MyClass {
  value = 10;
  constructor() {
    var addThis2 = function(first, second) {
      return this.value + first + second;
    }.bind(this);
    // THIS IS SAME
    var addThis3 = (first, second) => this.value + fist + second;
  }
}
```

- 화살표 함수는 익명 함수다.
- 화살표 함수는 콜백 함수의 `this` 범위로 생기는 오류를 피하기 위해 자동으로 `bind()` 함수를 사용하여 `this` 객체를 전달한다. 
- 이로 인해 직관적으로 **상위 스코프의 `this` 를 가리키게 된다.**(이를 `Lexical this` 라 한다.)

<details>
<summary style="cursor: pointer;">QUIZ: function vs arrow function</summary>
<div markdown="1">

기존의 함수는 `new` 키워드를 통해 인스턴스 생성을 할 수 있다. 하지만 화살표 함수는 "익명 함수"이며 인스턴스 생성을 할 수 없다.

`scope` 의 차이가 나기 때문에 바라보는 `this` 가 서로 다르다.

</div>
</details>

## 객체 확장 표현식과 구조 분해 할당

```javascript
const x = 0;
const y = 1;
const con = { x, y };

const str = `1ilsang`;
const obj = {
  [str + '-dev']: `1ilsang-dev`,
  go() {
    return 'wow';
  }
};
console.log(obj['1ilsang-dev'], obj.go());

const arr = [0, 1];
let [a, b] = arr;
[b, a] = [a, b];
console.log(a, b);

const obj2 = {
  a: 1,
  b: 2,
};
const {
  a: A,
  b,
  c = 'Default Value'
} = obj2;
console.log(A, b, c, obj2);

const [item1, ...otherItems] = [0, 1, 2];
const {key1, ...others} = { a: 1, b: 2, key1: 'KEY1' };
console.log(item1, otherItems, key1, others);
```

<details>
<summary style="cursor: pointer;">INFO: 구조 분해 할당에서의 유의점</summary>
<div markdown="1">

```javascript
const b = undefined;
const {
  a = 'default!'
} = b;
// ERROR! Cannot read property 'a' of undefined.
```

분해할 객체 자체가 없을 경우 에러가 발생한다. 자주 일어날 수 있으므로 값을 명확히 해주어야 한다.

</div>
</details>

## 라이브러리 의존성 관리

기존의 자바스크립트는 라이브러리를 `<script src="..." />` 엘리먼트를 이용해 관리했다.

- 이렇게 할 경우 **순서** 가 매우 중요해진다.(로드 되기 전의 스크립트 파일을 사용하면 에러)

위의 문제를 해결하고자 ES6 에서 `import` 구문을 통해 `script` 엘리먼트 없이 ***연결된 파일 및 의존 파일을 모두 먼저 내려 받고 코드를 구동*** 하도록 변경했다.

## 배열 함수

- `forEach, map, reduce`

`forEach` 가 요소 자체를 순회하는 것이라면 `map` 함수는 요소를 순회하면서 "반환한 값으로 새 배열을 만든다".

`reduce` 함수는 요소를 순회하면서 누적해서 값을 만들고, 최종적으로 누적된 값을 리턴한다.

```javascript
const arr = [1, 2, 3];
const obj = arr.reduce((acc, cur) => {
  acc.value += cur;
  return acc;
}, {value: 0});

console.log(obj, obj.value);
```

- `reduce` 함수는 주로 배열을 특정 자료형으로 변환하는데 사용한다.

<details>
<summary style="cursor: pointer;">QUIZ: forEach vs map</summary>
<div markdown="1">

forEach 는 리턴 없이 요소 호출만 하지만, map 의 경우 요소 호출마다 기존 배열과 동일한 사이즈의 새로운 배열을 반환한다.

이로인해 forEach 보다 map 이 70% 정도 느리다.

map 이 각광받는 이유는 다른 함수들과 "조합"이 가능하기 때문.

`arr.map(e => e * 2).filter(e => e % 2);` 와 같이 사용할 수 있다.

</div>
</details>

## 비동기 함수

ES6의 비동기 함수 처리 방법 알아보기

```javascript
const work1 = () =>
  new Promise(resolve => {
    setTimeout(() => resolve('작업1 완료!'), 100);
  });
const work2 = () =>
  new Promise(resolve => {
    setTimeout(() => resolve('작업2 완료!'), 200);
  });
const work3 = () =>
  new Promise(resolve => {
    setTimeout(() => resolve('작업3 완료!'), 300);
  });
function urgentWork() {
  console.log('긴급 작업');
}

const nextWorkOnDone = msg1 => {
  console.log('done after 100ms:' + msg1);
  return work2();
};

work1()
  .then(nextWorkOnDone)
  .then(msg2 => {
    console.log('done after 200ms:' + msg2);
    return work3();
  })
  .then(msg3 => {
    console.log(`done after 600ms:${msg3}`);
  });
urgentWork();
const work1and2 = () =>
  work1().then(msg1 => {
    console.log('done after 100ms:' + msg1);
    return work2();
  });

work1and2()
  .then(msg2 => {
    console.log('done after 200ms:' + msg2);
    return work3();
  })
  .then(msg3 => {
    console.log('done after 600ms:' + msg3);
  });
```

<details>
<summary style="cursor: pointer;">QUIZ: 위 코드의 실행 결과는?</summary>
<div markdown="1">

```javascript
// 긴급 작업
// done after 100ms: 작업1 완료!
// done after 100ms: 작업1 완료!
// done after 200ms: 작업2 완료!
// done after 200ms: 작업2 완료!
// done after 600ms: 작업3 완료!
// done after 600ms: 작업3 완료!
```

</div>
</details>

## 디바운스와 스로틀

- "지연 처리"에 대해 다룬다.
- 디바운스(debounce)는 "마지막에 입력된 내용"을 요청한다.
  - 예: 자동 완성(유저가 빠르게 타이핑하는 동안은 무시하다 잠깐 멈출 때 요청)
- 스로틀(throttle)은 "특정 시간의 첫 번째 내용"을 요청한다.
  - 예: 무한 스크롤(스크롤이 마지막 즈음으로 갈 때 최초 한 번 요청하고 일정 시간동안 액션 홀딩)

무한 스크롤에서 디바운스를 사용하게 될 경우 유저가 계속해서 스크롤을 내리고 있다면 무한히 Delay 만큼의 여유가 있는 마지막 입력을 기다리게 되므로 새로운 페이지가 뜨지 않게된다.

```javascript
// Debounce
// inDebounce 를 기준으로 timeout 을 거는 형태.
// 입력이 계속해서 들어오면(return function 이 실행되면) clearTimeout 으로 timeout 을 지움.
// 이로 인해 delay 만큼의 충분한 시간이 지난 경우에만 실행 됨.
export function debounce(func, delay) {
  let inDebounce;
  return function(...args) {
    if(inDebounce) clearTimeout(inDebounce);
    inDebounce = setTimeout(() => func(...args), delay);
  }
};

const run = debounce(val => console.log(val), 100);
run('a');
run('b');
run(2);
// After 100ms
// 2
```

```javascript
// Throttle
function throttle(func, delay) {
  let lastFunc;
  let lastRan;
  return function(...args) {  // 아래 코드의 window.addEventListener 의 인자가 args 로 들어오게 된다.
    const context = this;     // this hold
    if(!lastRan) {            // 최초의 실행에 해당. 즉시 함수를 실행시키며 현재 시간을 lastRan 에 넣어준다.
      func.call(context, ...args);
      lastRan = Date.now();
    } else {
      // 마지막에 입력받은 타이머가 있다면 클리어 시킨다.(딜레이 까지 모두 무시)
      if(lastFunc) clearTimeout(lastFunc);
      lastFunc = setTimeout(function() {
        // 현재 시간과 이전에 실행시킨 시간을 뺀 값과 delay 를 비교해 실행 시기를 정한다.
        if((Date.now() - lastRan) >= delay) {
          func.call(context, ...args);
          lastRan = Date.now();
        }
      }, delay - (Date.now() - lastRan));
    }
  }
}

const checkPosition = () => {
  const offset = 500;
  const currentScrollPosition = window.pageYOffset;
  const pageBottomPosition = document.body.offsetHeight - window.innerHeight - offset;
  if(currentScrollPosition >= pageBottomPosition) { // 바텀의 근처로 스크롤이 진입하면
    // fetch('/page/next');
    console.log('다음 페이지 로딩');
  }
};

const infiniteScroll = throttle(checkPosition, 300); // 첫 이벤트 시작 후 300ms 동안의 이벤트는 무시하는 throttle 함수를 가져온다.
window.addEventListener('scroll', infiniteScroll); // 스크롤 이벤트가 일어날 때마다 infiniteScroll 함수를 실행시킨다.
```
