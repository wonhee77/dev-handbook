# Reflection

JVM은 클래스 정보를 클래스 로더를 통해 읽어와서 해당 정보를 JVM 메모리에 저장한다.  이 저장된 클래스에 대한 정보가 거울에 투영된 모습과 닯아 있어 리플렉션이라는 이름을 가지게 되었다.

리플렉션을 사용해서 생성자, 메소드, 필드 등 클래스 정보를 가져올 수 있다.

### Class 객체 획득 방법

```java
Class<Member> aClass = Member.class; // 클래스의 class 프로퍼티를 사용

Member member1 = new Member();
Class<? extends Member> bClass = member1.getClass(); // 인스턴스의 getClass() 사용

Class<?> cClass = Class.forName("hudi.reflection.Member"); // Class의 forName() static method 사용
```

### getXXX() vs getDeclaredXXX()

getXXX 과 같은 형태는 상속받은 클래스와 인터페이스를 포함하여 모든 public 요소를 가져온다.

getDeclaredXXX는 해당 클래스에 직접 정의된 내용만 가져온다.

### Constructor

Class를 사용해서 생성자를 Constructor 타입으로 가져올 수 있다.

```java
Constructor<?> constructor = aClass.getDeclaredConstructor();
Constructor<?> allArgsConstructor = aClass.getDeclaredConstructor(String.class, int.class);

Object object = constructor.newInstance();

Member member = (Member) constructor.newInstance();
```

### FIELD

```java
Class<Member> aClass = Member.class;
Member member = new Member("후디", 25);

for (Field field : aClass.getDeclaredFields()) {
    field.setAccessible(true);
    String fieldInfo = field.getType() + ", " + field.getName() + " = " + field.get(member);
    System.out.println(fieldInfo);
}
```

### METHOD

```java
Class<Member> aClass = Member.class;
Member member = new Member("후디", 25);

Method sayMyName = aClass.getDeclaredMethod("sayMyName");
sayMyName.invoke(member); // method의 invoke를 사용해서 직접 호출 가능
```

### ANNOTATION

```java
Class<Member> aClass = Member.class;

Entitiy entityAnnotation = aClass.getAnnotation(Entitiy.class);
String value = entityAnnotation.value();
System.out.println("value = " + value);
// 멤버
```

### 단점

리플렉션은 런타임에 클래스를 분석하므로 속도가 느리다.

타입 체크가 컴파일 타임에 불가능하다.

객체의 추상화가 깨진다.