# Validation
**`Validation`**은 `Bean Validation API이라는 자바의 표준 어노테이션`을 사용한다.  
Spring에서는 이 Bean Validation API를 통해 Controller로 들어오는 http 요청에 대해 validation 기능을 지원한다.  
'org.springframework.boot:spring-boot-starter-validation' 의존성을 추가하면  
jakarta의 'jakarta.validation:jakarta.validation-api:3.0.0' 그리고 스프링에서 선택한 구현체인  
'org.hibernate.validator:hibernate-validator:7.0.1.Final'를 같이 의존하게 된다.

http 요청에 대한 validation을 사용하려면 contorller에서 @RequestBody에 @Valid라는 어노테이션이 필요하고,  
DTO의 필드에 아래의 어노테이션을 붙여주면 된다.

##1. 종류
### 1. Null 관련
@Null(message="") // null만 허용  
@NotNull(message="") // null 허용 X,  "", " "는 허용  
@NotEmpty(message="") // null, "" 허용 X,  " "는 허용  
@NotBlank  (message="") // null, "", " " 허용 X  

### 2. 값 관련
@Size(min=, max= ,message="") // 데이터의 사이즈(최소 길이, 최대 길이) 설정  
@Max(value = ,message="") // value 이하의 값만 허용  
@Min(value = ,message="") // value 이상의 값만 허용  
@Positive(message="") // 값을 양수만 허용  
@PositiveOrZero(message="") // 값을 양수와 0만 허용  
@Negative(message="") // 값을 음수만 허용  
@NegativeOrZero(message="") // 값을 음수와 0만 허용  
@AssertTrue(message="") // True일 때만 허용(null 체크 X)  
@AssertFalse(message="") // False일 때만 허용(null 체크 X)  
@Digit(integer="", fraction="") // 숫자일 경우만 동작, integer는 최대 정수 자리수, fraction은 최대 소수 자리수 

### 3. 날짜 관련
@Future(message="") // 현재보다 미래의 날짜만 허용  
@FutureOrPresent(message="") // 현재와 미래의 날짜만 허용  
@Past(message="") // 현재보다 과거의 날짜만 허용  
@PastOrPresent(message="") // 현재와 과거의 날짜만 허용

### 4. 정규식
@Pattern(regexp = ,message="") // 정규식을 이용해서 검사






