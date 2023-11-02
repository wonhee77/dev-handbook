# Servlet

기존의 서버는 정적인 자료만 주고 받았지만 다양한 기능이 요구되면서 동적인 페이지들을 만들 필요가 생겼다. 이를 위해 서블릿이 만들어지게 되었다.

- 클라이언트의 요청에 동적으로 응답하는 웹 어플리케이션 컴포넌트
- HTML을 사용하여 응답
- JAVA의 쓰레드를 사용
- MVC 패턴의 컨트롤러 역할

# Dispatcher Servlet

HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러

과거에는 모든 서블릿을 URL 매핑을 위해 web.xml에 등록해주어야 했지만 dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고 공통 작업을 처리할 수 있게 되었다.

### 동작 방식



### 1. *HttpServlet*: Servlet의 Request, Response를 HttpServlet의 Request,Response로 변환


먼저 클라이언트의 요청은 DispatcherServlet의 부모 추상 클래스인 HttpServlet의 service() 메서드에서 요청을 받습니다.

위 코드의 13, 14라인에서 서블릿의 요청 및 응답을 HttpServlet의 요청 및 응답으로 변환합니다.

이후 18번째 라인의 service() 메서드는 *FrameworkServlet* 추상 클래스에서 오버라이드된 *service()* 메서드를 호출합니다.

### 2. *FrameworkServlet*: HTTP 메서드에 따라 분기 처리

Http Method가 PATCH일 경우 processRequest() 메서드를 호출하고, 그 외의 메서드는 14라인의 super.service() 메서드를 호출합니다. (super = HttpServlet)

현재 요청한 Http Method는 GET이므로 14라인의 super.service() 메서드를 호출하여 다시 HttpServlet 클래스의 service() 메서드를 호출합니다.

### 3. *HttpServlet*: service()

HttpServlet 추상 클래스의 service() 메서드는 Http Method에 따라 *doXXX* 메서드를 호출합니다.

실제 호출은 HttpServlet 클래스를 상속받고, doXXX 메서드를 오버라이딩하고 있는 자식 클래스의 doXXX 메서드입니다.

HttpServlet 추상 클래스에서 템플릿을 제공하고, 실제 비즈니스 로직은 이를 상속받는 FrameworkServlet에서 구현되어 있는데요, 이는 디자인 패턴에서 템플릿 메서드 패턴이 적용되었음을 확인할 수 있습니다.

### 4. *FrameworkServlet*: doXXX() -> processRequest() -> doService()

자식 클래스에서 오버라이딩된 doXXX 메서드는 위와 같이 모두 *processRequest()* 메서드를 호출하도록 되어있습니다.

FrameworkServlet 추상 클래스의 processRequest() 메서드에서 핵심 로직은 7번째 라인의 doService() 입니다.

doService() 메서드가 드디어 DispatcherServlet 클래스를 호출하는 진입점입니다!

그럼 DispatcherServlet의 doService() 메서드에서는 어떠한 과정이 일어나는지 살펴보겠습니다.

### 5. *DispatcherServlet*: doService()

해당 메서드에서 가장 중요한 부분은 16번째 라인의 *doDispatch()* 메서드인데요, 이 함수에서 아래의 작업을 진행합니다.

- 요청에 매핑되는 HandlerMapping(HandlerExecutionChain) 조회
- Handler을 수행할 수 있는 HandlerAdapter 조회
- HandlerAdapter를 통해 Controller 호출 (실제 로직)

### 6. *DispatcherServlet*: doDispatch() - HandlerMapping, HandlerAdapter, Handle Controller

doDispatch() 메서드는 위에서 말씀드렸듯이 세 가지 핵심 작업을 진행 합니다.

- 18 라인: HandlerMapping(HandlerExecutionChain, 전후처리 기능 포함) 조회
- 26라인: Handler을 수행할 HandlerAdapter 조회
- preHandler
- 45라인: HandlerAdapter를 통해 Controller의 실제 로직 호출
- postHandler

6-1. 요청에 매핑되는 HandlerMapping(HandlerExecutionChain) 조회

6-2. Handler을 수행할 수 있는 HandlerAdapter 조회 *(28번째 라인)*

*getHandlerAdapter()*: HandlerExecutionChain를 통해 *HandlerAdapter*를 찾습니다.

예제 코드에서 작성했던 @GetMapping 어노테이션 내부에 @RequestMapping 어노테이션이 존재하기 때문에 위의 for문에서는 RequestMappingHandlerAdapter 어댑터를 반환합니다.