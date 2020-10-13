# ECMAScript Grammar

[toc]

## 0. ECMAScript?

**`ECMAScript`는 [자바스크립트](https://developer.mozilla.org/ko/docs/Web/JavaScript)의 토대를 구성하는 스크립트 언어**이다. ECMAScript는 [ECMA International](http://www.ecma-international.org/) 표준화 기구에 의해서 ECMA-262 및 ECMA-402 스펙에서 표준화되었다.



## 1. 변수와 식별자

### 1-1. 식별자

변수명은 `식별자`라고 불리며 특정 규칙을 따른다.

- 반드시 **문자, 달러, _로 시작**해야 하며 숫자로는 시작할 수 없다. (`-`은 아예 사용할 수 없다.)
- **대소문자를 구분**한다. 클래스명이 아니라면 시작 단어는 대문자 사용을 지양한다.
- **예약어도 사용 불가능**하다. (class, const, let, var, await, case 등..)



#### 식별자 작성 스타일

- 기본적으로 `camelCase`로 작성한다. (lowcaseUppercase)

```javascript
// 숫자, 문자, 불린
let dog
let babyCat

// 배열 - 복수형으로 작성한다.
let animals

// 함수
function getPropertyName () {}

// 이벤트 핸들러 - 'on'으로 시작한다.
function onClick () {}
function onKeyDown () {}

// 불린 반환 함수(리턴 값이 true, false) - 'is'로 시작한다.
function isAuthenticated () {}
```



- `PascalCase`로 작성되는 경우 - 클래스, 생성자

```javascript
class User {
    information(option) {
        this.name = option.name
    }
}

const people = new User({
    name: '김현지',
})
```



- `SNAKE_CASE`로 작성되는 경우 - 값이 변하지 않는다는 것을 알린다.

```javascript
const API_KEY = 'api_key'
const PI = '3.141592...'
```



### 1-2. 변수

#### let(변수): 값을 재할당할 수 있는 변수를 선언하는 키워드

```javascript
let x = 1
x = 3
```

- **선언은 단 한 번만** 가능하다.

```javascript
let x = 3
let x = 2
```



- 유효 범위가 블록을 기준으로 작성된다.
- 블록 유효 범위란? `if`, `for` 등 `{} 내부`를 가리킨다.

```javascript
let x = 1
if (x == 1) {
    let x = 2
    console.log(x) // 결과값: 2
}
console.log(x) // 결과값: 1
```



#### const(상수): let보다 조금 더 엄격하다. 재할당도 재선언도 불가능하다.

- **값이 변하지 않는 상수를 선언**하는 키워드

```javascript
const x = 4
x = 3
// Uncaught TypeError: Assignment to constant variable. at <anonymous>:1:3
console.log(x) // 결과값: 4

const foo = {}
foo.bar = 4
console.log(foo)
console.log(foo.bar)
```



- 상수를 선언할 때는, **반드시 값을 초기화**해주어야 한다.

```javascript
// 불가능
const a

// 반드시 어떤 값을 넣어주어야지만 설정이 가능하다.
const a = 3
```



- let과 같이 블록 유효 범위를 가진다.

```javascript
const num = 3

if (num == 3) {
    const num = 4
    console.log(num)
}
console.log(num)
```



#### var(변수): ES6 이전에 사용하던 변수 선언 방식

- 재할당, 재선언 둘 다 가능하다.
- 그러나 예기치 못한 상황을 발생시킬 수 있으므로 `절대 사용하지 않는다`.

```javascript
var x = 1
var x = 2
console.log(x)
```



- 함수 유효 범위를 가진다.

```javascript
var x = 33
let y = 11

if (x == 33){
    var x = 22
    let y = 1
    console.log(x) // 22
    console.log(y) // 1
}
console.log(x) // 22
console.log(y) // 11
```



#### 정리

| const       | let         | var            |
| ----------- | ----------- | -------------- |
| 재할당 불가 | 재할당 가능 | 재할당 가능    |
| 재선언 불가 | 재선언 불가 | 재선언 가능    |
| block scope | block scope | function scope |



### 1-3. Hoisting

- `var`로 선언한 변수가 **선언 이전에 참조**될 수 있는 현상
- 우리가 활용하는 기술이 아닌 **피해야 하는 상황**이다.

```javascript
console.log(name)

var name = '김현지'
```

python이었다면 NameError가 떴겠지만 JavaScript에서는 에러가 나지 않는다.

```javascript
var name
console.log(name)

var name = '김현지'
```

실제로 코드를 실행할 때는 위처럼 이해하고 작동하기 때문이다. 



- let으로 작성했을 시

```javascript
console.log(name)

let name = '김현지'
// const name = '김현지'
```





## 2. 타입과 연산자

### 2-1. Primitive(원시값, 원시 자료형) 타입

#### Number

```javascript
const a = 1
const b = -13
const c = 3.14
const d = 2.99e3
const e = Infinity
const f = -Infinity
const g = NaN

console.log(a, b, c, d, e, f, g)
```



#### String

```javascript
const sentence1 = '문자열'
const sentence1 = "문자열"

console.log(sentence1)
console.log(sentence2)
```

- 문자열 간의 덧셈이 가능하다.

```js
const firstName = '현지'
const lastName = '김'

const fullName = firstName + lastName
console.log(fullName)
```

- escape sequence

```javascript
const hello = "안녕 \n 하세요"
console.log(hello)
```

- Template Literal

```javascript
const name = '김현지'

const sayHello = `${name}가 인사합니다.`
console.log(sayHello)
```

- escape sequence를 사용할 수 없다.

```javascript
const nextSentence = `안녕
하세요`
console.log(nextSentence)
```



#### Boolean

```javascript
true
false
```



#### Empty Value

- **값이 존재하지 않음**을 나타내는 방식에는 두 가지가 있다.
- `undefined`: 값을 할당하지 않았을 때 **JavaScript가 자동으로 할당**해주는 값

```javascript
let name
console.log(name)
```

- `null`: **값이 없음**을 나타내기 위해 사용한다.

```javascript
let name = null
console.log(name)
```

```javascript
typeof null
"object"

typeof undefined
"undefined"
```

`JS null object`: 개발자의 실수로 인해 평생 object로 남게 되었다..



### 2-2. 연산자

#### 할당 연산자

- 연산과 동시에 **변수에 어떤 값을 할당**하는 연산자

```javascript
let c = 10

c += 10
console.log(c)

c -= 10
console.log(c)

c *= 10
console.log(c)

c /= 10
console.log(c)

c++ // 증강식 1을 더해주는 역할
console.log(c)

c-- // 증감식 1을 빼주는 역할
console.log(c)
```



#### 비교 연산자

- 두 값을 비교하기 위해 사용하며, `true` or `false`를 반환한다.

```javascript
3 > 2 // true
3 < 2 // false
```

- 문자열도 비교가 가능하다.
  - 영어 소문자가 대문자보다 큰 값을 가진다.
  - 알파벳은 오름차순으로 비교한다.

```javascript
'A' < 'a'
```



#### 동등 연산자 (==)

- 형 변환했을 때 같은 값이면 일치한다고 본다.
- JS는 **자동으로 형 변환**을 해준다.
- 자동 형 변환으로 인한 예기치 못한 상황이 발생할 수 있다. 그러므로 **동등 연산자 사용을 지양**한다.

```javascript
const a = 1
const b = '1'

console.log(a == b)
console.log(a == Number(b))
```



#### 일치 연산자 (===)

- 동등 연산보다 **엄격한 비교**를 한다.

```javascript
const a = 1
const b = '1'

console.log(a === b)
console.log(a === Number(b))
```



#### 논리 연산자

- `and` &&

```javascript
true && true
true && false

0 && 0 // 0
1 && 0 // 0
4 && 7 // 7 (단축평가)
```



- `or` ||

```javascript
true || false
false || true

1 || 0 // 1
4 || 7 // 4 (단축평가)
```



- `not` ! (반대로 출력)

```javascript
!true // false
!false // true
```



#### 삼항 연산자

- 가장 앞의 조건식이 참이면 `:`을 기준으로 앞에 작성한 값이 반환되고, 반대의 경우 `:` 뒤의 값이 반환된다.

```javascript
true ? 1 : 2 // 1
false ? 1 : 2 // 2
```

```javascript
const result = 3.14 > 4 ? 'Yes' : 'No'
console.log(result) // No
```





## 3. 조건문과 반복문

### 3-1. 조건문

- `if`

```javascript
const name = 'kim'

if (name === 'kim') {
    console.log('True')
}
```

- `else if`

```javascript
const name = 'kim'

if (name === 'lee') {
    console.log('LEE')
} else if (name === 'kim') {
    console.log('KIM')
} else {
    console.log(`${name}`)
}
```



#### switch

- 하나의 변수의 값에 따라 분기를 하는 조건문이다.

```javascript
const name = '김현지'

switch (name) {
    case 'admin': {
        console.log('ADMIN')
    }
    case 'manager': {
        console.log('MANAGER')
    }
    default: {
        console.log(`${name}`)
    }
}
```

- `case`에 해당하는 값이 있다면, 그 내용을 실행하고 그 다음도 실행하면서 default에 정의한 내용도 함께 실행된다.
- `break`와 함께 사용한다.

```javascript
const name = '김현지'

switch (name) {
    case 'admin': {
        console.log('ADMIN')
        break
    }
    case 'manager': {
        console.log('MANAGER')
        break
    }
    default: {
        console.log(`${name}`)
    }
}
```



### 3-2. 반복문

#### while

- `while` 키워드 뒤에 오는 조건이 `true`인 동안 반복한다.

```javascript
let i = 0
while (i < 6) {
    console.log(i)
    i++
}
```



#### for

- `for문`에서는 항상 `let`으로 선언한다.

```javascript
for (let i = 0; i < 6; i++) {
    console.log(i)
}
```



#### for of

- 배열에서 **요소를 하나씩 순회하며 반복**하는 반복문이다.

```javascript
const numbers = [1, 2, 3, 4]

for (const number of numbers) {
    console.log(number) // 1, 2, 3, 4
}
```

근데 왜 여기서는 `const`를 쓸까? -> `block` 내({} 안)에서만 변수로 선언되기 때문이다.



#### for in

- **object의 key 값을 순회**하는 반복문이다.
- 배열의 경우 **index 값을 순회**한다.

```javascript
const numbers = [1, 2, 3, 4]

for (const number in numbers) {
    console.log(number) // 0, 1, 2, 3
}

const fruits = {
    a: 'apple',
    b: 'banana'
}

for (const key in fruits) {
    console.log(key) // a, b
    console.log(fruits[key]) // apple, banana
}
```





## 4. Function

### 4-1. functions

#### 선언식

```javascript
function add (num1, num2) {
    return num1 + num2
}

add(1, 3)
```

`{} 안`에 함수가 실행할 행동이 들어간다.



#### 표현식

- 일반적으로 표현식은 `익명함수`로 작성한다.
- 익명함수는 표현식에서만 사용 가능하다.

```javascript
const sub = function (num1, num2) {
    return num1 - num2
}

sub(3, 1)
```

```javascript
const mySub = function namedSub (num1, num2) {
    return num1 - num2
}

mySub(3, 1) // 2
namedSub(3, 1) // Uncaught ReferenceError: namedSub is not defined
```

오류가 발생했을 때 `namedSub` 함수의 이름이 나온다.



#### 기본 인자

```javascript
const greeting = function (name = '김현지') {
    return name
}
```



### 4-2. 선언식 vs 표현식

```javascript
console.log(typeof add)
console.log(typeof add)
```



#### Hoisting

- 위에서와 마찬가지로 `Hoisting`은 활용할 게 아니라 주의해야 한다!

```javascript
add(2, 7)

function add (num1, num2) {
    return num1 + num2
}
```

- `표현식`은 함수를 변수에 할당했고, 일종의 변수처럼 작동한다.

```javascript
sub(3, 1)

const sub = function (num1, num2) {
    return num1 - num2 // Uncaught ReferenceError: Cannot access 'sub' before initialization
}
```

- `var`로 함수 선언시

```javascript
var sub
sub(3, 1)

var sub = function (num1, num2) {
    return num1 - num2 // Uncaught TypeError: sub is not a function
}
```



### 4-3. Arrow Function

function과 중괄호를 생략하여 코드를 줄이기 위해 고안된 단축 문법 `=>`

- `function 생략` 가능
- 함수의 매개변수가 하나 밖에 없다면 `() 생략` 가능
- 함수의 바디 {} 안에 들어가는 표현식이 하나면 `{} return 생략` 가능

```javascript
const arrow = function (name) {
    return `${name}입니다.`
}

// 1. function 생략
const arrow = (name) => {
    return `${name}입니다.`
}

// 2. () 생략
const arrow = name => {
    return `${name}입니다.`
}

// 3. {} return 생략 가능
const arrow = name => `${name}입니다.`
arrow('김현지')
```





## 5. Data Structure

### 5-1. array

- 사용법

```javascript
const numbers = [1, 2, 3, 4]

numbers[0]
numbers[3]
// numbers[-1] -> undefined 출력
```



#### 자주 사용하는 메서드

- `push` && `pop`

```javascript
numbers.push('5') // 5 출력 -> 새로운 배열의 길이
numbers.pop() // '5' -> 가장 마지막 요소 삭제
```



- `reverse`: 배열을 반대로 정렬

```javascript
numbers.reverse() // [4, 3, 2, 1]
```



- `unshift` && `shift`

```javascript
numbers.unshift('a') // 5 -> 새로운 배열의 길이
numbers // ['a', 1, 2, 3, 4]

numbers.shift() // 'a' -> 가장 처음 요소
numbers // [1, 2, 3, 4]
```



- `includes`: true / false `boolean` 값으로만 반환한다.

```javascript
numbers.includes(1) // true
numbers.includes(5) // false
```



- `indexOf`: 배열에 특정 요소가 있는지 확인하고, 있다면 가장 첫 번째로 찾은 요소의 `index`를 반환한다. 없다면 `-1`을 반환한다.

```javascript
numbers.indexOf(1) // 0
numbers.indexOf(0) // 없으므로 -1 반환

numbers.push('a')
numbers.push('a')
numbers // [1, 2, 3, 4, 'a', 'a']
numbers.indexOf('a') // 4 -> 중복된 값이 있다면 가장 첫 번째로 찾은 요소의 index 반환
```



- `join`

```javascript
numbers.join() // "1, 2, 3, 4, a, a"
numbers.join('') // "1234aa"
numbers.join('-') // "1-2-3-4-a-a"
```



### 5-2. Object

- 사용법
- **key 값은 문자열 형태**로 작성, **value는 모든 형태**가 작성될 수 있다.

```javascript
const person = {
    name: '김현지',
    'phone number': '010-1234-5678',
    pocket: {
        phone: 'iPhone',
        earphone: 'AirPods',
        'laptop': 'SAMSUNG'
    }
}
```



#### 요소 접근

- dot notation

```javascript
person.name // "김현지"
person['name'] // "김현지"

person['phone number'] // "010-1234-5678"

person.pocket.phone // "iPhone"
person.pocket['phone'] // "iPhone"
```



#### Object 축약 문법

- 객체를 정의할 때 key 값과 할당하는 변수의 이름이 같으면 축약이 가능하다.

```javascript
const books = ['python', 'JavaScript']

const professor = {
    professor1: ['django', 'JavaScript'],
    professor2: ['python', 'Java']
}

const anything = null

const school = {
    books,
    professor,
    anything
}

console.log(school)
```



```javascript
const userInformation = {
    name: 'hyunji'
    userId: 'khyunji'
}
```

- ES5 이전

```javascript
const name = userInformation.name
const userId = userInformation.userId
```

- ES6+ 이후

```javascript
const { name } = userInformation
const { userId } = userInformation
```



```javascript
function getUserInfo (userInformation) {
    console.log(`userName = ${userInformation.name}`)
}

getUserInfo(userInformation)

function getuserInfo ({ name }) {
    console.log(`userName = ${name}`)
}
getUserInfo(userInformation)
```



### 5-3. JSON (JavaScript Object Notation)

- key -> value처럼 생겼지만 실제로는 **문자열**
- Object처럼 사용하려면 `Parsing` 작업이 필요하다.



#### Object -> JSON(string)

```javascript
const jsonData = JSON.stringify({
    tea: '녹차',
    coffee: '라떼'
})

console.log(jsonData) // {"tea":"녹차","coffee":"라떼"}
console.log(typeof jsonData) // string
```



#### JSON(srting) -> Objects(parsing)

```javascript
const parseData = JSON.parse(jsonData)

console.log(parseData) // {tea: "녹차", coffee: "라떼"}
console.log(typeof parseData) // object
```



#### 5-4. Array Helper Method

- `helper` 일종의 라이브러리

- ES5.1 버전에 처음 등장. 본격적으로 사용된 것은 ES6부터.



#### forEach

- 주어진 `callback`의 배열에 있는 **각 요소를 오름차순으로 한 번씩 실행**한다.

```javascript
arr.forEach(callBack(element, index, array))
```
- 예시

```javascript
const colors = ['red', 'blue', 'violet']

colors.forEach(function (color) {
    console.log(color)
})

colors.forEach(color => console.log(color))
```

- `forEach`는 return 값이 없다.

```javascript
const result = colors.forEach(color => console.log(color))
console.log(result) // undefined
```



#### map

- 배열의 모든 요소에 대해 `callback`을 실행한다. 함수의 반환값을 요소로 하는 새로운 배열을 반환한다.
- 배열을 **내가 원하는 다른 형식으로 바꿔야할 때** 쓴다.

```javascript
arr.map(callback(element, index, array))
```

- 예시

```javascript
const numbers = [1, 2, 3]

const mapNumbers = numbers.map(function (number) {
    return number * 2
})

console.log(mapNumbers) // [2, 4, 6]
```



#### filter

- `callback` 함수를 실행했을 때, 어떠한 테스트 혹은 조건을 만족하고 통과하는 모든 요소를 모아서 **새로운 배열을 반환**한다.

```javascript
arr.filter(callback(element, index, array))
```

- 예시

```javascript
const products = [
    { name: 'cucumber', type: 'vegetable' },
    { name: 'banana', type: 'fruit' },
    { name: 'carrot', type: 'vegetable' },
    { name: 'apple', type: 'fruit' },
]

const fruits = products.filter(function (product) {
    return product.type === 'fruit'
})

console.log(fruits)
```



#### reduce

- 배열 내의 **숫자 총합, 평균 계산 시**에 사용
- 계산된 값을 배열의 하나의 값으로 줄이는 동작

```javascript
arr.reduce(callback(acc, element, index, array), initialValue)
```

- 첫번째 매개변수 `acc`는 누적 값을 의미한다.
- 두번째 매개별수 `initialValue`는 초기 값을 의미한다.

```javascript
const tests = [90, 90, 80, 77]

const sum = tests.reduce(function (total, x) {
    return total + x
}, 0)  // 여기서 0일 때는 0 생략 가능

console.log(sum)
```



#### find

- `callback`을 만족하는 첫번째 요소를 찾아서 반환한다. 없으면 `undefined`를 반환한다.

```javascript
arr.find(callback(element, index, array))
```

- 예시

```javascript
const avengers = [
    { name: 'Tony Stark', age: 45 },
    { name: 'Steve Rogers', age: 32 },
    { name: 'Thor', age: 40 },
]

 const avenger = avengers.find(function (avenger) {
     return avenger.name === 'Tony Stark'
 })
 
console.log(avenger)
```



#### some

- 배열 안의 요소 중, **단 하나라도 조건을 만족한다면** `true` 반환, 아니면 false 반환

```javascript
arr.some(callback(element, index, array))
```

- 예시

```javascript
const arr = [1, 2, 3, 4, 5]
const result = arr.some(elem => elem % 2 === 0)
console.log(result) // true

const arr = [1, 3, 5]
const result = arr.some(elem => elem % 2 === 0)
console.log(result) // false
```



#### every

- 배열 안의 요소가 **모두 조건을 만족**해야 `true` 반환, 아니면 false 반환

```javascript
arr.every(callback(element, index, array))
```

- 예시

```javascript
const arr = [1, 2, 3, 4, 5]

const result2 = arr.every(elem => elem % 2 === 0)  // false
```
