# StringBuilder, StringBuffer
String은 Immutable한 객체이다. 따라서 String 타입의 데이터를 다룰 일이 많을 때 String 객체를 사용하면  
계속 새로운 객체를 만들어야되어서 비효율적이다.  
이때 StringBuffer, StringBuilder를 사용하면 문자열과 관련된 연산을 더 효율적으로 진행할 수 있다.  

## 1. String   
`String` 클래스는 불변성(Immutable)을 가지는 문자열을 나타낸다. 한번 `String` 객체가 생성되면, 그 값은 변경할 수 없다.   
`String` 객체에 대해 변경 작업(예: concatenation, substring 등)을 수행하면 새로운 `String` 객체가 생성된다.   
이는 메모리 및 성능에 비효율적일 수 있다. 그러나 `String`의 불변성은 멀티스레드 환경에서 안전성을 보장하므로,여러 스레드에서 동시에 접근해도 안전하다.    
`Thread-safe`한 `불변` 객체

## 2. StringBuilder  
`StringBuffer` 클래스는 가변성(Mutable)을 가지는 문자열을 표현한다.   
`StringBuffer` 객체에 대해 변경 작업을 수행하면 새로운 객체를 생성하는 대신 기존 객체가 직접 변경됩니다.  
이는 메모리 사용과 성능에 있어 `String`보다 효율적이다.  
또한 `StringBuffer`는 내부적으로 동기화되어 있어, 멀티스레드 환경에서도 안전하다.   
하지만 이 동기화는 단일 스레드 환경에서는 불필요한 오버헤드를 초래할 수 있다.  
`Thread-safe`한 `가변` 객체

## 3. StringBuffer  
`StringBuilder` 클래스도 가변성(Mutable)을 가지는 문자열을 표현한다.   
`StringBuffer`와 같이 `StringBuilder` 객체에 대한 변경 작업은 기존 객체를 직접 변경한다.   
그러나 `StringBuilder`는 `StringBuffer`와 달리 동기화되어 있지 않다.   
따라서 `StringBuilder`는 단일 스레드 환경에서 `StringBuffer`보다 더 높은 성능을 제공한다.   
그러나 멀티스레드 환경에서는 안전하지 않을 수 있다.  
`Thread-safe`하지 않은 `가변` 객체

## 4. 정리

`String`은 불변하고 스레드에 안전하며,   
`StringBuffer`와 `StringBuilder`는 가변적이지만,   
`StringBuffer`는 스레드에 안전하고, `StringBuilder`는 그렇지 않다.   
따라서 문자열이 빈번하게 변경되는 시나리오에서는 `StringBuffer`나 `StringBuilder`를 사용하는데   
멀티스레드 환경에서는 `StringBuffer`를,  
단일 스레드 환경에서는 `StringBuilder`를 사용하는 것이 좋다.  
