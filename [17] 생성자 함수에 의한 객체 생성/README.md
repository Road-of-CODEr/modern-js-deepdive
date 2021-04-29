# 생성자 함수란?

생성자 함수는 new 연산자와 함께 호출해서 객체를 생성할 수 있는 함수를 말한다. 이런 생성자 함수를 사용해서 인스턴스(객체)를 생성할 수 있다.

# 객체 리터럴 방식이 아니라 생성자 함수를 사용하는 이유

아래는 객체 리터럴 방식을 통해서 객체를 생성하는 경우인데, 이때 동일한 프로퍼티를 가진 객체를 여러개 생성할 때 같은 객체 리터럴 방식으로 프로퍼티를 기술해야하는 점이 비효율 적이다. 또한 state와 같은 프로퍼티는 객체마다 다를 수 있지만 `getState`와 같은 메서드는 내용이 공통인 경우가 있는데, 이런 메소드 선언이 반복되는 것이 문제가 될 수 있다. 
```javascript
const obj = {
    state: 'state',
    getState() {
        return this.state;
    }
}
```

생성자 함수는 객체를 생성하는 템플릿처럼 이런 생성자 함수를 통해서 프로퍼티 구조가 동일한 인스턴스를 쉽게 반복적으로 생성할 수 잇따. 

# 생성자 함수 사용방법

생성자 함수는 함수에 new 연산자와 함께 호출하는 방식으로 동작한다. new 연산자가 없으면 일반 함수와 동일하게 동작해서 함수내에서 this를 사용한다면 이때의 this는 window(브라우저) 객체이다. 

```javascript
function func1() {
  this.propertyOfFunc1 = 'hello'
}

// 생성자, 이때의 this는 생성될 인스턴스
const obj = new func1();
console.log(obj.propertyOfFunc1)

// 일반 함수호출, 이때의 this는 window
func1();
console.log(window.propertyOfFunc1)
```

이때 생성자 함수가 new 연산자 없이 호출되는 것을 막지 않으면 예기치 않게 전역객체에 프로퍼티가 할당 될수 있다. 이를 방지하기 위해서 `new.target`을 사용할 수 있다.

`new.target`은 생성자로 호출된 함수내에서는 `자기자신`을 가르키지만, 일반함수로 호출되는 경우에는 `undefined` 값을 가진다. 

```javascript
function func1() {
    if(!new.target){
        return new func();
    }
    this.propertyOfFunc1 = 'hello'
}
```

이를 통해서 생성자로 호출 되지 않은 경우에도 인스턴스를 생성해서 반환한다. 되지

# 생성자 함수를 통해서 인스턴스가 생성되는 과정

```javascript
function Sample(state){
    // 초기화
    this.state = state;
    this.getState = function() {
        return this.state
    }
}

const Sample = new Sample(1); // 런타임 이전에 암묵적으로 빈객체를 생성하고 this 바인딩을 한다. 런타임에 실제로 값을 초기화한다. 
```

위와 같은 생성자 함수는 인스턴스를 생성하고, 초기화하고 생성한 인스턴스를 반환한다. 상세한 과정은 아래와 같다. 
1. 인스턴스를 생성하고, this에 바인딩 된다. 
    - 생성자 함수가 실행되면 암묵적으로 빈객체가 생성되고, 이가 이 생성자 함수의 this에 바인딩이 된다. 
    - 런타임 이전에 instance가 생성되고, this가 바인딩 된다. 
    - 런타임때는 인스턴스가 초기화 되고 인스턴스를 반환한다. 
2. 인스턴스 초기화  
   - 생성자 함수 내의 코드가 실행되고, 암묵적으로 생성된 인스턴스에 해당 값을 정의한다. 

3. 암묵적으로 this를 반환한다. 

# Ref
- 모던 자바스크립트 딥다이브
