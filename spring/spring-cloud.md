# 모놀리스(Monolith) 아키텍처

- 모든 업무로직을 하나의 애플리케이션 형태로 묶어 서비스 하는 형태
- 단일 단위로 디자인 되고 개발 배포된다
- 모놀리스 아키텍처의 특성상 구성 요소에 기능을 추가하거나 수정하는 것은 큰 비용을 초래

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98169ab5-afb5-4022-b3f1-cef18c3681e9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98169ab5-afb5-4022-b3f1-cef18c3681e9/Untitled.png)

- 단일 코드 베이스는 개발팀의 분리부터 그에 대한 관리, 프로덕션 배포까지 시간이 흐를 수록 복잡도는 증가한다
- 전체 코드에 대한 이해도는 떨어지고 하위 기능으로 내려갈 수록 코드의 복잡성은 더욱 증가한다
- 새로운 기술 적용에도 전체 애플리케이션이 영항을 받기 때문에 쉽게 반영하기 어렵다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce812010-79d8-4b2d-9cdc-ba7f707af35a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce812010-79d8-4b2d-9cdc-ba7f707af35a/Untitled.png)

→ 이를 개선하기위해 나온 구조가 SOA (Service Oriented Architecture) 이다 (SOA(서비스 지향 아키텍처)는 서비스 단위로 독립된 서비스로 구성하여 서로 커뮤니케이션(SOAP)을 통해 동작하는 구조를 뜻한다)

### SOA vs Micro Service Architecture

- SOA 는 SOAP(p - protocol) 을 통해 통신
- SOA는 분산처리 환경에서 발생하는 문제를 해결하기 위해 ESB(Enterprise Service Bus)를 사용
- MSA는 REST api 를 통해 통신

ESB는 여러 서비스 노드와 연결되어 부하가 집중되지만 MSA는 각 서비스간 통신에 필요한 독립적인   연결이 구성된다. 즉, SOA는 단 하나의 ESB로 통신, MSA는 각 통신에 필요한 독립적인 ESB로 구성 (다수의 ESB)로 구성된다. MSA는 서비스간 통신을 구성하는 것이 복잡하고, 장애 추적이나 전파를 방지하기 어렵기 때문에 서비스 매쉬(istio)라는 아키텍처를 사용하게된다.

# 마이크로서비스 아키텍처

- 소규모의 독립적인 구성요솔 구분하여 개발하는 방식
- 디자인, 개발, 배치, 관리되는 잘 정의된 비즈니스 기능을 제공
- 모노리딕한 서비스를 작은 단위의 서비스로 모듈화하고 서로 HTTP 기반으로 커뮤니케이션하도록 구성하는 것을 의미
- 개념적으로는 SOA와 큰 차이가 없고 객체지향의 개념과도 유사하다

```markdown
"마이크로 서비스 아키텍처 스타일은 단일 응용 프로그램을 나누어 작은 서비스 조합으로 구축하는 
방법이며, 각 개별 서비스는 자신의 프로세스에서 실행하는 HTTP 기반 API 등으로 가벼운 연결 
방식을 사용한다" 

- 마틴 파울러-
```

- 근래의 애플리케이션은 규모가 크고 내용이 복잡하기 때문에 한 개인이 전체를 이해하고 개발하기 매우 어렵다
- 개발자가 서비스를 이해하고 개발할 수 있는 모듈 단위로 분해하고 각 서비스는 다른 서비스의 접근을 위한 API 경계선을 가지게 되고 이를 통해 각 서비스는 견고한 모듈성을 갖는다
- 각 서비스별 별도의 기술 스택과 독립적인 배포/확장이 가능하도록 한다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96dfa02e-01e3-41f6-93af-6150cfce00d9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96dfa02e-01e3-41f6-93af-6150cfce00d9/Untitled.png)

### 장점

- 작은 코드 베이스를 가지므로 관리가 용이하다
- 개별적인 컴포넌트로 스케일 관리가 쉽다 (특정 서비스에 대한 스케일 아웃이 쉬워짐)
- 기술에 대한 다양성을 가질 수 있다 (캡슐화, 서비스 간의 커뮤니케이션 규약만 지킬 수 있다면 내부적인 기술이나 구성은 제약이 없다)
- 결함을 고립하여 전체 시스템의 다운을 방지할 수 있다
- 동시에 작업하는 더 작은 개발팀을 구성할 수 있다
- 독립적인 배포가 가능하다

### 단점

- 서비스간의 강한 일관성(ACID)를 달성하기 어렵다
- 분산 환경이므로 디버깅과 추적이 어렵다
- End to End 테스트의 필요성이 증가한다
- 애플리케이션 개발과 배포 방법(CI/CD)이 중요해진다
- 서비스간 호출에 따른 커뮤니케이션 비용이 증가한다
- 다수의 서비스가 서로 협력하여 사용자에게 서비스를 제공하기 때문에 서비스간 소통에 대한 비용이 발생한다
- 하나의 기능을 수행하는데 다수의 서비스가 협업하기 때문에 오류 지점을 특정하기 어렵다

→ 이러한 단점을 개선하며 MSA를 제공하는 기술중 하나가 Spring Cloud이다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8936cf80-6677-4655-8c44-a143aaca146f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8936cf80-6677-4655-8c44-a143aaca146f/Untitled.png)

# API 게이트웨이

![https://docs.microsoft.com/ko-kr/dotnet/architecture/microservices/architect-microservice-container-applications/media/direct-client-to-microservice-communication-versus-the-api-gateway-pattern/custom-service-api-gateway.png](https://docs.microsoft.com/ko-kr/dotnet/architecture/microservices/architect-microservice-container-applications/media/direct-client-to-microservice-communication-versus-the-api-gateway-pattern/custom-service-api-gateway.png)

클라이언트에서 MSA 구조의 서비스에게 각각 요청을 하게되면 여러가지 문제가 발생하게 될수 있다. 클라이언트-MSA 의 직접 통신이 아닌 클라이언트-API게이트웨이-MSA 의 구조로 구성하는 이유는 다음과 같다

- **인증/인가 문제(교차 편집 문제)**: 공개적으로 게시된 각 마이크로서비스는 권한 부여 및 SSL과 같은 문제를 처리해야 한다. 대부분의 경우 단일 계층에서 처리할 수 있어 내부 마이크로 서비스가 간소화 된다 (이전에는 모든 마이크로 서비스가 동일하게 중복된 로직을 가져야 했지만, API 게이트 웨이가 있다면 쉽게 해결 가능하다)
- **결합**: 클라이언트는 마이크로 서비스 엔드포인트에 대한 여러 호출을 관리하고 처리해야 한다 (내부 엔드포인트와 결합되어 유지보수하기 어려워진다)
- **너무 많은 왕복**: 클라이언트의 한 페이지에서 여러 서비스를 호출해야 할수도 있다. 이 경우 클라이언트 서버간의 여러번의 네트워크 왕복이 발생할 수 있고, 대기시간이 크게 증가할 수 있다. API 게이트웨이가 있다면 캐싱이나 클라이언트 환경보다 빠르고 강력한 컴퓨팅파워로 대기시간을 크게 향상시킬 수 있다.
- **보안 문제**: 외부에 노출되는 엔드포인트가 줄어들어 공격 표면이 작아지는 효과가 생긴다

Zuul

Spring Cloud Gateway

[스프링 부트 강의 - 3-1강 Monolith 아키텍처의 이해](https://www.youtube.com/watch?v=-JHAUaOq6l0)

[API 게이트웨이 패턴과 클라이언트-마이크로 서비스 간 직접 통신](https://docs.microsoft.com/ko-kr/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern)
