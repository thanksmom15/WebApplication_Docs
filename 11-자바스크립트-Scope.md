# Scope

### 스코프
* 변수의 유효 범위

```
var a = 10;
function scope1(){
    a = 20;
    console.log(a); // 20
}
scope1();
console.log(a); // 20

var b = 10;
function scope2(){
    var b = 20;
    console.log(b); // 20
}
scope2();
console.log(b); // 10
```

### 변수 참조 과정
* LHS 탐색에 의해 가까운 스코프 내의 변수에 할당
    * `scope1` 함수 실행 : `scope1` 스코프 내에는 a라는 변수가 없으므로 다음으로 가까운 global 스코프에서 a를 찾음
    * `scope2` 함수 실행 : `scope2` 스코프 내에는 b라는 변수가 있으므로 자신의 스코프에 있는 변수 b에 값을 할당

* RHS 검색에 의해 console.log에서 각 변수를 참조
    * `scope1`의 console.log : `scope1` 스코프 내에는 a라는 변수가 없으므로 다음으로 가까운 global 스코프에서 a를 찾음
    * global의 console.log : 자신의 스코프에 있는 변수 a를 찾음
    * `scope2`의 console.log : `scope2` 스코프 내에는 b라는 변수가 있으므로 자신의 스코프에 있는 변수 b을 찾음

### var / let / const
* var : 함수 스코프에서 유효
	* 선언 전 사용 가능
* let : 블록 스코프에서 유효
	* 이미 선언되어있는 이름과 같은 이름 선언 불가
* const : 블록 스코프에서 유효
    * 선언과 동시에 할당이 이루어져야 하며, 재할당 불가
    * C의 define, JAVA의 final과 같은 맥락
    * db환경정보, api 응답값 등 변하지 않을 값을 담을 때 사용

```
if(true){
  var a = 'hi';
}
console.log(a); // hi

if(true){
  let b = 'bye';
}
console.log(b); // ReferenceError: b is not defined
```

# Hoisting

### 호이스팅
* 실행되기 전 함수 안에 필요한 변수값들을 모두 모아서 유효 범위의 최상단에 선언하는 것
    * 선언과 할당의 분리
    * 유효 범위 : 함수 블록 안에서 유효

### 대상
* `변수 선언`과 `함수 선언문`에서만 호이스팅이 일어남
* 선언만 위로 올려지며, 할당은 올려지지 않음

### 변수 선언 예시
* var
	* 호이스팅 되면서 초기값이 없으면 자동으로 undefined를 초기 값으로 하여 메모리 할당
```
console.log(num); // undefined
var num;
```
</br>

* let
	* 호이스팅 되면서 초기값이 없다면 var처럼 자동으로 초기값을 할당하지 않음
	* const의 경우 선언시 초기값을 할당하지 않으면 문법 에러
	* 값이 할당되기 전까지 메모리를 할당하지 않기 때문에 사용하려고 하면 해당변수가 존재하지 않아 에러 발생
	* TDZ(Temporal Dead Zon) : 변수가 선언되고 해당 변수에 값이 할당되지 전까지
```
console.log(num); // Uncaught ReferenceError: num is not defined
let num = 1;
console.log(num); // 1
```
```
/** --- JS Parser 내부의 호이스팅의 결과 --- */
let num;
console.log(num); // 값이 할당되기 전이므로 아직 num은 메모리에 존재하지 않음. TDZ.
num = 1;
console.log(num); // 1
```


### 함수 선언문 예시
* 함수 선언문은 위치와 관계없이 호이스팅에 따라 맨 위로 올려짐
```
function printName(firstname) { // 함수선언문 
    var result = inner(); // "선언 및 할당"
    console.log(typeof inner); // > "function"
    console.log("name is " + result); // > "name is inner value"

    function inner() { // 함수선언문 
        return "inner value";
    }
}

printName(); // 함수 호출
```
```
/** --- JS Parser 내부의 호이스팅의 결과 --- */
function printName(firstname) { 
    var result; // [Hoisting] var 변수 "선언"

    function inner() { // [Hoisting] 함수선언문
        return "inner value";
    }

    result = inner(); // "할당"
    console.log(typeof inner); // > "function"
    console.log("name is " + result); // > "name is inner value"
}

printName();
```

### 함수 표현식 예시
* 함수 선언문과 달리 선언과 호출 순서에 따라서 정상적으로 함수가 실행되지 않을 수 있음
    * 함수 표현식에서는 선언과 할당의 분리가 발생
</br>

* 함수 표현식의 선언이 호출보다 위에 있는 경우
    * 정상 출력
```
function printName(firstname) { // 함수선언문
    var inner = function() { // 함수표현식 
        return "inner value";
    }
        
    var result = inner(); // 함수 "호출"
    console.log("name is " + result);
}

printName(); // > "name is inner value"
```
```
/** --- JS Parser 내부의 호이스팅의 결과 --- */
function printName(firstname) { 
    var inner; // [Hoisting] 함수표현식의 변수값 "선언"
    var result; // [Hoisting] var 변수값 "선언"

    inner = function() { // 함수표현식 "할당"
        return "inner value";
    }
        
    result = inner(); // 함수 "호출"
    console.log("name is " + result);
}

printName(); // > "name is inner value"
```
</br>

* 함수 표현식의 선언이 호출보다 아래에 있는 경우 (var 변수에 할당)
    * TypeError
    * `printfName`이 실행되는 순간 호이스팅에 의해 `inner`는 undefined으로 지정
    * `inner`가 undefined라는 것은 아직 함수로 인식이 되지 않고 있다는 것을 의미
```
function printName(firstname) { // 함수선언문
    console.log(inner); // > "undefined": 선언은 되어 있지만 값이 할당되어있지 않은 경우
    var result = inner(); // ERROR!!
    console.log("name is " + result);

    var inner = function() { // 함수표현식 
        return "inner value";
    }
}
printName(); // > TypeError: inner is not a function
```
```
/** --- JS Parser 내부의 호이스팅의 결과 --- */
function printName(firstname) { 
    var inner; // [Hoisting] 함수표현식의 변수값 "선언"

    console.log(inner); // > "undefined"
    var result = inner(); // ERROR!!
    console.log("name is " + result);

    inner = function() { 
        return "inner value";
    }
}
printName(); // > TypeError: inner is not a function
```
</br>

* 함수 표현식의 선언이 호출보다 아래에 있는 경우 (let/const 변수에 할당)
    * ReferenceError
    * console.log에서 inner에 대한 선언이 되어있지 않았기 때문에 참조에러 발생
```
function printName(firstname) { // 함수선언문
    console.log(inner); // ERROR!!
    let result = inner();  
    console.log("name is " + result);

    let inner = function() { // 함수표현식 
        return "inner value";
    }
}
printName(); // > ReferenceError: inner is not defined
```

### 호이스팅 우선순위
* 같은 이름의 var 변수 선언과 함수 선언에서의 호이스팅
    * 변수 선언이 함수 선언보다 위로 올려짐
```
var myName = "hi";

function myName() {
    console.log("yuddomack");
}
function yourName() {
    console.log("everyone");
}

var yourName = "bye";

console.log(typeof myName);
console.log(typeof yourName);
```
```
/** --- JS Parser 내부의 호이스팅의 결과 --- */
// 1. [Hoisting] 변수값 선언 
var myName; 
var yourName; 

// 2. [Hoisting] 함수선언문
function myName() {
    console.log("yuddomack");
}
function yourName() {
    console.log("everyone");
}

// 3. 변수값 할당
myName = "hi";
yourName = "bye";

console.log(typeof myName); // > "string"
console.log(typeof yourName); // > "string"
```
</br>

* 값이 할당되어 있지 않은 변수와 값이 할당되어 있는 변수에서의 호이스팅
    * 값이 할당되어 있지 않은 변수의 경우 : 함수 선언문이 변수를 덮어씀
    * 값이 할당되어 있는 변수의 경우 : 변수가 함수 선언문을 덮어씀
```
var myName = "Heee"; // 값 할당 
var yourName; // 값 할당 X

function myName() { // 같은 이름의 함수 선언
    console.log("myName Function");
}
function yourName() { // 같은 이름의 함수 선언
    console.log("yourName Function");
}

console.log(typeof myName); // > "string"
console.log(typeof yourName); // > "function"
```

### 주의
* 변수명 중복 사용을 지양!!
* 코드의 가독성과 유지보수를 위해 호이스팅이 가급적 일어나지 않도록 해아함
* 함수와 변수를 가급적 코드 상단부에 선언
