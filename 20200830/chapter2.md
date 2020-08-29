리액트 ES6 문법 액기스
---

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
<summary style="cursor: pointer; color: blue">QUIZ: 템플릿 리터럴 vs 표현식 삽입법 vs 태그된 템플릿 vs Raw string</summary>
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
<summary style="cursor: pointer; color: blue">QUIZ: 두 객체를 병합할 때 중복된 키값들은 어떻게 되는가?</summary>
<div markdown="1">

뒤의 값이 앞의 값을 덮어쓴다.

</div>
</details>

## 가변 변수와 불변 변수

- let vs const

<details>
<summary style="cursor: pointer; color: blue">QUIZ: Immutable 에 대해 let 과 const 를 엮어 설명해 보시오.</summary>
<div markdown="1">

let 은 가변이며 const 는 불변이다.

이때의 "불변"은 주소 값에 대한 불변이므로 해당 주소 안의 데이터(값)은 변경이 된다.

따라서 `const arr = [];` 와 같이 const 로 선언된 배열에 push 등으로 값을 변경할 수 있다.

Immutable 은 "불변"으로 주로 `Object.freeze` 와 같이 더이상 값 또한 변할 수 없는 상태를 뜻한다.(freeze 또한 완벽한 불변을 줄 순 없다)

</div>
</details>

<details>
<summary style="cursor: pointer; color: blue">QUIZ: 두 객체를 병합할 때 중복된 키값들은 어떻게 되는가?</summary>
<div markdown="1">

뒤의 값이 앞의 값을 덮어쓴다.

</div>
</details>

<details>
<summary style="cursor: pointer; color: blue">QUIZ3: 가변 내장 함수와 무결성 내장 함수를 선택하시오</summary>
<div markdown="1">

(push, concat, slice, splice, pop, slice)

|가변 함수|무결성 함수|
|push|concat|
|splice|slice|
|pop|slice|

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
<summary style="cursor: pointer; color: blue">QUIZ: static method 의 장단점을 설명해 보시오</summary>
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

