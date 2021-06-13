# 객체

> [poiemaweb | 5.11 객체와 변경불가성](https://poiemaweb.com/js-immutability).
> [MDN | Object.freeze()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze).

## 1. 객체 생성 방법

### 1-1. 객체 리터럴

중괄호 `{}`를 사용하여 객체를 생성하는 방식이다.

### 1-2. Object 생성자 함수

new 연산자와 Object 생성자 함수를 호출하여 빈 객체를 만든 후 새로운 프로퍼티와 메소드를 추가하는 방식이다.
사실 객체 리터럴 방식으로 객체를 생성하는 것이 Object 생성자 함수로 객체를 생성하는 것의 단축 표현이다.

```javascript
var obj = new Object();
```

### 1-3. 생성자 함수

```javascript
// 생성자 함수
function Person(name, gender) {
  var married = ture; // private
  this.name = name; // public
  this.gender = gender;
  this.greeting = console.log(`Hi! My name is ${name}.`);
}

// 인스턴스 생성
var person1 = new Person("kim", "female");
var person2 = new Person("lee", "male");
```

- this에 연결되어 있는 프로퍼티와 메소드는 `public`(외부에서 참조 가능)하다.
- 생성자 함수 내에서 선언된 일반 변수는 `private`(외부에서 참조 불가능)하다.

생성자 함수가 아닌 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다. 따라서 일반적으로 생성자 함수명은 첫문자를 대문자로 기술하여 혼란을 방지하려는 노력을 한다.

new 연산자와 함께 함수를 호출하면 `this` 바인딩이 다르게 동작한다.

---

<br>

## 2. 프로퍼티 접근

### 프로퍼티 키

프로퍼티 키는 따옴표 `""`로 감싼 문자열로 지정해야 한다. 하지만 자바스크립트에서 변수명으로 사용 가능한 이름인 경우 따옴표를 생략할 수 있다.
표현식을 프로퍼티 키로 사용할 경우 대괄호 `[]`로 묶어 사용할 수 있다.

```js
const str1 = "num";
const str2 = "ber";

let obj1 = {
  name: "obj",
  its_number: 1,
  "its-type": "object",
  1: 2,
  [str1 + str2]: 3,
};

obj1.number; // 3
```

### 2-1. 프로퍼티 값 읽기

- 마침표 표기법 `.`
- 대괄호 표기법 `[]`

`[]`안에는 따옴표로 묶인 문자열 형식의 키, 숫자 타입의 키, **변수**가 들어갈 수 있다.

```js
const str3 = str1 + str2;

obj1.name  // "obj"
obj1["name"]  // "obj"
obj1[name]  // undefined

obj1.its-type  // error
obj1['its-type'] // "object"
obj1.1  // error
obj1[1] // 2
obj1.str3 // undefined
obj1[str3]  // 3
```

### 2-2. 프로퍼티 삭제

- delete  
  delete 키워드의 피연산자는 객체의 프로퍼티 `key`이어야 한다.

```javascript

```

---

<br>

## 3. pass-by-reference

객체는 변경 가능한 값이다.

---

<br>

## 4. 불변 데이터 패턴

<br>
변경불가성(immutablity)은 객체를 생성하고 난 후 변경이 불가능하도록 만드는 것을 의미한다.

객체는 참조(reference)형태로 전달하고 전달받으며 원시 타입과 달리 변경 가능한 값이다. 객체의 참조 값을 공유하는 형태이기 때문에 객체의 값을 변경했을 때 참조하고 있는 다른 모든 장소에서 변경이 일어나게 된다.

이 때 의도치 않은 변경을 방지하기 위한 것이 불변 데이터 패턴이다. 객체 자체를 불변 객체로 만들거나, 객체의 변경이 필요할 경우 복사본을 만들어 값을 변경한다. 정리하면 다음과 같다.

1. 불변 객체화  
   Object.freeze
2. 객체의 방어적 복사 (defensive copy)  
   Object.assign

<br>

### 4-1. 불변 객체화

> Object.freeze(obj)  
> Object.deep

`.freeze()`는 객체를 변경할 수 없게 동결한다. 동결된 객체는 프로퍼티의 값을 변경할 수도, 프로퍼티를 새롭게 추가, 삭제할 수도 없다. 하지만 deep freeze하지 않기 때문에 객체 안의 객체(Nested Object)는 변경 가능하다.
매개변수로 동결할 객체를 받으며 함수에 전달한 객체를 그대로 반환한다.
동결 후 변경을 시도하면 조용히 무시되지만 엄격 모드에서는 타입 에러가 발생한다.

```javascript
const user1 = {
  name: "lee",
  address: {
    city: "seoul",
  },
};
Object.freeze(user1);

user1.name = "kims";
user1.address.city = "Busan";

console.log(Object.isFrozen(user1)); // true
console.log(user1.name); // 'lee'
// deep freeze되지 않는다.
console.log(user1.address.city); // 'Busan'

// 배열 동결
let arr = [1, 2];
Object.freeze(arr);

arr[0] = 3;
arr.push(3); // TypeError

console.log(Object.isFrozen(arr)); // true
console.log(arr); // [1, 2]
```

<br>

### 4-2. 객체의 방어적 복사

> Object.assign(target, ...sources)

ES6에서 새로 추가됨  
첫 번째 매개변수를 target 객체로 인식  
타깃 객체에 소스 객체의 복사본을 결합시키는 형태

`.assign()`은 deep copy하지 않기 때문에 객체 안의 객체, nested object는 shallow copy된다.  
즉, 방어적 복사를 해도 내부 객체는 보호되지 않는다.

```javascript
const user1 = {
  name: "lee",
  address: {
    city: "seoul",
  },
};
const user2 = Object.assign({}, user1);
// 다른 레퍼런스를 참조한다.
console.log(user1 === user2); // false

user2.name = "kim";
user2.address.city = "Busan";

console.log(user1.name); // 'lee'
console.log(user2.name); // 'kim'
// 내부 객체는 shallow copy된다.
console.log(user1.address.city); // 'Busan'
console.log(user2.address.city); // 'Busan'
```

### 4-3. immutable.js

- 모르는 것  
  Object.set()

---

## 5. 객체 순회

객체는 `length` 속성이 없기 때문에 `for`반복문으로 순회는 불가능하다. 대신 다음과 같은 방법이 있다.

### 5-1. Object.keys()

> 어떤 객체가 가지고 있는 키들의 목록을 배열로 리턴하는 메소드

```js
const obj = {
  name: "melon",
  weight: 4350,
  price: 16500,
  isFresh: true,
};
const keys = Object.keys(obj); // ['name', 'weight', 'price', 'isFresh']

for (let i = 0; i < keys.length; i++) {
  const key = keys[i]; // 각각의 키
  const value = obj[key]; // 각각의 키에 해당하는 각각의 값
  console.log(value);
}
```

### 5-2. for ...in

> for (let _key_ in _obj_)

for ...in 반복문은 객체의 key를 순회한다. 배열에도 쓸 수 있는데 1. 배열의 순서를 보장하지 않고 2. 요소만 반환하는 것이 아니기 때문에 배열에는 사용하지 않는 것이 좋다.

```js
//배열에 사용한 경우 (오류)------------
const arr = ["a", "b", "c"];
arr.name = "array";

for (let idx in arr) {
  console.log(`${idx} : ${arr[idx]}`);
}
/*
0 : a
1 : b
2 : c
name : array
*/

// 객체 순회----------------------
const obj = {
  name: "melon",
  weight: 4350,
  price: 16500,
  isFresh: true,
};

for (let key in obj) {
  const value = obj[key];

  console.log(`${key} : ${value}`);
}
/*
name : melon
weight : 4350
price : 16500
isFresh : true
*/
```
