# JavaScript Types
: 자바스크립트 언어는 `primitive type`과 `object type`이 있다.
### Primitive Type
* undefined: `typeof instance==="undefined"`
* null: `typeof instance==="object"`
* Boolean: `typeof instance==="boolean"`
* Number: `typeof instance==="number"`
* String: `typeof instance==="string"`
* BigInt: `typeof instance==="bigint"`
* Symbol: `typeof instance==="symbol"` (ECMAScript6에 추가됨)
### Object Type
* Object: `typeof instance==="object"`

---


### 원시타입/기본타입 (Primitive Type)
: 객체가 아니면서 메서드도 가지지 않는 데이터  
* 원시 값에는 7종류가 있다. `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, `null`  
* 모든 원시 값은 불변하다.  

``` javascript
// 문자열 메서드는 문자열을 변형하지 않는다.
var bar = "hello";
console.log(bar); // hello
bar.toUpperCase();
console.log(bar); // hello --> HELLO 아님!

// 할당은 원시값에 새로운 값을 부여한다.
bar = bar.toUpperCase(); // HELLO
```
* null과 undefined를 제외하고 모든 원시값은 래퍼객체가 있다.
  * **래퍼객체**: new연산자와 생성자함수를 굳이 사용하지 않아도 변수에 해당 값을 입력하면 객체로 인식하는 객체  
  * 따라서 new 연산자로 String 객체를 생성하지 않았더라도 toUpperCase와 같은 String 표준 내장 객체의 메서드를 사용할 수 있다. 
  원시값이며 불변값으로 생성되었지만, 임시로 생성된 래퍼객체에서 메서드가 수행되기 때문이다.


#### BigInt Type
: BigInt를 사용하면 Number의 정수 한계를 넘어서는 큰 정수도 안전하게 저장 및 연산할 수 있다.  
상수 `Number.MAX_SAFE_INTEGER`를 사용하여 숫자로 증가시킬 수 있는 가장 안전한 값을 얻을 수 있다.  

#### Symbol Type
: 주로 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티 키(property key)를 만들기 위해 사용한다.  
`unique` and `immutable` primitive value  
symbol을 이용하여 자바스크립트에서 `enum` 개념 사용 가능하다.



<br/><br/>

[참고](https://another-light.tistory.com/105)