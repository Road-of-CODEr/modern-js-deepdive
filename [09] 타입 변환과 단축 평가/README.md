자바스크립트의 타입 변환에는 `암묵으로 타입 변환(타입 강제 변환)`하는 방법과 `명시적으로 타입 변환(타입 캐스팅)`하는 방법 두가지가 있다.

# 암묵적 타입 변환
## 문자열 타입 변환
```js
-0 + ''
-1 + ''
Infinity + ''
-Infinity + ''
(Symbol())
({}) + ''
Math + ''
Array + ''
[] + ''
[10, 20] + ''
(function(){}) + ''
```

# 숫자 타입으로 변환
산술 연산자(+, -, %, /)의 역할은 숫자 값을 만드는 것.
비교 연산자(>, <)의 역할은 불리언 값을 만드는 것.

이 두 연산자는 숫자 값/불리언 값을 만들기 위해 모든 피연산자는 코드 문맥상 숫자타입 이여야 한다. 
```js
+null
+undefined

+''
+{}
+[]
+[10, 20]
+(function(){})
+'string'
```

# 불리언 타입으로 변환
제어문 또는 삼항 조건 연산자의 `조건식`은 논리적 참/거짓으로 평가할 수 있는 표현식이어야 하기 떄문에 불리언 이어야 한다.

아래 Falsy값 빼고 나머지는 다 Truthy 값으로 판단.

false로 평가되는 Falsy값은? (6개)
```js
false
undefined
null
0, -0
NaN
''
``` 

출력되는 결과는?
```js
console.log( [] ? true : false );
console.log( {} ? true : false );
```

## 명시적 타입 변환

방법
1. 표준 빌트인 생성자 함수를 new 연산자 없이 호출
> 표준 빌트인 생성자?
> - 객체를 생성하기 위한 함수, new 연산자와 함께 호출
> - ex) String, Number, Boolean

2. 표준 빌트인 메서드를 사용하는 방법
> 표준 빌트인 메서드?
> - 자바스크립트에서 기본 제공하는 빌트인 객체의 메서드
> - ex) parseInt, parseFloat


### 문자열 타입으로 변환하는 방법
1. String 생성자 함수 new 연산자 없이 호출
2. Object.prototype.toString 메서드 사용
3. 문자열 연결 연산자

### 숫자 타입으로 변환
1. Number 생성자 함수를 new 연산자 없이 호출
2. parseInt, parseFloat 함수 사용 (문자열만 사용 가능)
3. 산술연산자 사용

### 불리언 타입으로 변환
1. Boolean 생성자 함수를 new 연산자 없이 호출
2. ! 연산자 두번 사용

```js
Boolean('false');
Boolean(NaN);
Boolean(Infinity);
!!{};
!![];

```


# 단축 평가
단축 평가는 표현식을 평가하는 도중, 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 의미한다.

단축 평가를 사용하면 if문을 대체할 수 있다.

`논리 연산자, 옵셔널 체이닝, null 병함 연산자`를 이용하여 단축 평가를 하는 방법을 알아보자.
(예제를 보면 더 이해하기 쉬울 것!)

## 논리연산자
논리합(||) 또는 논리곱(&&) 연산자는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환.

```js
'Cat' && 'Dog'
'Cat' || 'Dog'
```

### UseCase1) 객체의 property 참조 전에 객체 null 혹은 undefined 체크
```js
let elem = null;
let value = elem && elem.value;
```

### UseCase2) 함수 매개변수에 기본값 설정
```js
function getStringLength(str) {
  str = str || '';
  return str.length;
}

// 근데 ES6 에서는 이렇게 하면 간단함 😅
function getStringLength(str = '') {
  return str.length;
}
```

### 논리 할당 연산자
[ES12(ECMAScript2021)](https://chanyeong.com/blog/post/29)에서 Logical assignment operators (논리 할당 연산자) 라는게 생김.

```js
// before
obj.prop = obj.prop || foo(); // obj.prop이 잘못된 값일 경우 할당
obj.prop = obj.prop && foo(); // obj.prop이 올바른 값일 경우 할당
obj.prop = obj.prop ?? foo(); // obj.prop이 null이나 undefined일 경우 할당

// after
obj.prop ||= foo();
obj.prop &&= foo();
obj.prop ??= foo();
```

## 옵셔널 체이닝 연선자(optional chaining)
- 연산자 `?.`
- ES11(ECMAScript2020)에서 도입
- 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환, 그렇지 않은 경우 `우항의 프로퍼티 참조`
- 📍 논리곱(&&) 연산자와 다른점
  - 좌항 피연산자가 Falsy값 이더라도 `null` 또는 `undefined`가 아니면 우항의 프로퍼티 참조를 이어감.
  ```js
  let str = '';

  let length = str?.length;
  console.log(length);
  ```

## null 병합 연산자(null coalescing)
- 연산자 `??`
- ES11(ECMAScript2020)에서 도입
- 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환, 그렇지 않은 경우 좌항의 피연산자를 반환
- 기본값 설정시 유용
- 📍 논리합(||) 연산자와 다른점
  - 좌항 피연산자가 Falsy값 이더라도 `null` 또는 `undefined`가 아니면 피연산자를 반환.
  ```js
  let foo1 = '' || 'default string';
  let foo2 = null ?? 'default string';
  ```