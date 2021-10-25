
### Write Once, Run Anywhere?
일반적으로 컴파일 언어(ex. C++, C, Swift)는 _(소스 코드 → 컴파일 → 기계어)_ 운영체제(플랫폼)에 종속적입니다.  
하지만 자바는 컴파일로 얻은 결과물이 .class 파일로 _(바이너리 코드(기계어)가 아닌 **바이트 코드**)_ JVM이 바이트 코드를 읽어주어 OS에 상관없이 실행됩니다.  

<br/>

### 가비지 컬렉션 garbage collection (GC)
더 이상 사용하지 않는 메모리를 자동으로 정리하는 기능입니다.  
_GC는 우리가 언제 발생하는 지 알 수 없습니다. 그저 언젠간 이뤄진다는 걸 알아두면 됩니다..!_

<br/>

### 자바 기본형 데이터 타입(Primitive Type)과 참조형 데이터 타입(Reference Type)
* **기본형 데이터 타입 Primitive Type**  

미리 정해진 크기의 메모리 사이즈로 표현되며 변수 자체에 값이 저장됩니다.  
byte(1 byte), short(2 byte), int(4 byte), long(8 byte), float(4 byte), double(8 byte), Boolean, char(2 byte)  
총 8가지 입니다.  
타입의 표현 범위가 커지는 방향으로 할당할 경우, 묵시적 형변환이 발생합니다.  
  * byte → short → int → long → float → double  
  * char → int 

명시적 형변환 같은 경우는 값 손실이 발생할 수 있으므로 개발자 책임 하에 형변환 진행할 수 있으며,  
묵시적 형변환 같은 경우는 자료의 손실 걱정이 없어 JVM이 서비스 해줍니다.

<br/>

* **참조형 데이터 타입 Reference Type**  

미리 정해질 수 없는 데이터를 표현하며 변수에는 실제 값을 참조할 수 있는 주소만 저장합니다.  
new 키워드 실행 시, 변수에는 실제 값을 참조할 수 있는 참조 값이 들어갑니다.  
참조형은 기본 데이터 타입을 제외한 모든 데이터 타입입니다.  
보통 String, int[], 사용자가 정의한 타입 등이 있습니다.  


<br/>

### switch 문에서 dobule은 사용 불가능하다.  
- byte, short, char, int 가능  
- enum 가능
- class object (Byte, Short, Character, Integer, String) 가능  
- method 호출 가능  

<br/>

### 객체 지향 프로그래밍 (Object Oriented Programming)
블록 형태의 모듈화된 프로그래밍 👉 추가, 삭제, 수정 용이하며 재사용성이 높습니다.    
현실세계에 있는 객체가 갖고 있는 속성과 기능은 추상화(abstraction)되어 클래스에 정의됩니다.  
클래스는 구체화되어 프로그램의 객체(instance, object)가 됩니다.  
* 클래스: 객체를 정의해 놓은 것, 객체의 설계도/틀 이라고 보면 됩니다. _붕어빵 틀_
* 객체: 메모리에 생성된 데이터로 클래스를 구체화한 것입니다. _붕어빵_

* instance vs object? 나누기 애매하지만.. 굳이 나누자면!  
  * object: 클래스 타입으로 선언되었을 때 (구현할 대상)
  * instance: object가 메모리에 할당되었을 때 (구현된 실체)

<br/>



