[우아한 테크 세미나 이동욱 스프링 배치](https://www.youtube.com/watch?v=_nkJkWVH-mo&t=1432s)

## Batch

- 배치 처리는 컴퓨터에서 사람과 상호작용 없이 이어지는 프로그램(작업)들의 실행이다

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7161d1fb-202e-48ba-b541-3c206f3c18a3/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7161d1fb-202e-48ba-b541-3c206f3c18a3/Untitled.png)

- Web Application 은 사용자와 상호작용이 지속적으로 이루어진다 (실시간 처리 / 상대적인 속도 / QA 용이성)
- Batch Application 은 한번 요청 후 상호작용 없이 내부적으로 완결된다 (후속 처리 / 절대적인 속도 / QA 복잡성) → Batch는 반드시 테스트 코드를 작성해야함 (QA가 힘들기 때문)

---

## Spring Batch 와 Quartz

- Quartz는 **스케줄링 프레임워크**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33b2d106-7247-426f-8f25-0a25b7504291/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33b2d106-7247-426f-8f25-0a25b7504291/Untitled.png)

### Batch Application 이 필요한 상황

- 일정 주기로 실행되어야 할때 (기간이 한달과 같이 긴 경우 대용량 데이터 처리가 절대적인 요구사항이 된다)
- 실시간 처리가 어려운 대량의 데이터를 처리할 때

- Spring Batch에서는 모든 데이터를 메모리에 쌓지 않는 조회 방식이 기본이다 (findAll 과 같은 것은 피해야 함)

---

## Job / Step / Tasklet

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3689c0f4-5f78-416f-b137-7cbdf524661e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3689c0f4-5f78-416f-b137-7cbdf524661e/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/abfda475-29e0-4999-a0c8-c552b8499a2e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/abfda475-29e0-4999-a0c8-c552b8499a2e/Untitled.png)

Tasklet 은 인터페이스, ChunkOrientedTasklet은 Tasklet의 구현체 (청크단위로 처리하도록)

---

## @JobScope / @StepScope / JobParameter

Spring Batch는 외부에서 파라미터를 주입받아 Batch 컴포넌트에서 사용할 수 있다.

→ 이를 JobParameter라고 한다

```java
@Bean
public Job scopeJob() {
	return jobBuilderFactory.get("scopeJob")
					.start(scopeStep1(null))
					.next(scopeStep2())
					.build();
}

@Bean
@JobScope
public Step scopeStep1(@Value("#{jobParameters[requestDate]}") String requestDate) {
	return stepBuilderFactory.get("scopeStep1")
					.tasklet((contribution, chunkContext) -> {
							log.info(">>>> This is scopeStep1");
							log.info(">>>> requestDate = {}", requestDate);
							return RepeatStatus.FINISHED;
					})
					.build();
}

// 외부에서 JobParameter 가 주입되어서 작동됨
```

Spring Batch의 JobParameter는 Long / String / Double / Date 타입을 지원하지만, Enum / LocalDate / LocalDateTime 은 지원 안된다.

→ @Value 의 특성을 이용해서 해결

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/883dfa7f-5e22-48e1-8844-010f5bfd206f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/883dfa7f-5e22-48e1-8844-010f5bfd206f/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6064b917-ba7d-4eef-b4c0-2d82249b97e9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6064b917-ba7d-4eef-b4c0-2d82249b97e9/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99de0f69-1bf2-4687-bc49-b8f350b30e1b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/99de0f69-1bf2-4687-bc49-b8f350b30e1b/Untitled.png)

### JobScope, StepScope는 Late Binding (늦은 할당)

즉시 생성되지 않고 Job이 실행될때, Step이 실행될때 생성된다.

애플리케이션 실행 후에도 동적으로 reader / processor / writer bean 생성이 가능하다.

JobParameter 값에 따라 Reader와 Processor를 교체

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f04b578-0c4e-402f-8d14-ad380aedc939/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f04b578-0c4e-402f-8d14-ad380aedc939/Untitled.png)

---

# Spring Batch 활용

### 관리 도구들

- Cron (원시적인 방법)
- Spring MVC + API Call (좋지 못한 방법)
- Spring Batch Admin (Deprecated)
- Quartz + Admin
- CI Tools (Jenkins / Teamcity 등등)

- Jenkins 가장 추천!

- Integration (Slack, Email)
- 실행 이력 / 로그 관리 / Dashboard
- 다양한 실행 방법 (Rest API / 스케줄링 / 수동 실행)
- 계정 별 권한 관리
- 파이프라인
- Web UI + Script 둘다 사용 가능
- Plugin (Ansible, Github, Logentries 등)

## Spring Batch 실행

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/363c8492-b3ec-4e8f-b297-41877e0b2a63/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/363c8492-b3ec-4e8f-b297-41877e0b2a63/Untitled.png)

- Jenkins에 스크립트로 커맨드 입력하면 된다

### 공통되는 설정 관리

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0895cb8c-a696-40d4-b804-cab18e3d9436/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0895cb8c-a696-40d4-b804-cab18e3d9436/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b5d3f9f-521a-42d0-a749-872a29d1d3bb/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b5d3f9f-521a-42d0-a749-872a29d1d3bb/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcc445f5-4845-4230-a02f-19d54ff2a174/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcc445f5-4845-4230-a02f-19d54ff2a174/Untitled.png)

---

## 무중단 배포

웹서비스 무중단 배포는 대부분 [Blue-Green 배포](https://www.redhat.com/ko/topics/devops/what-is-blue-green-deployment)를 사용한다. 

→ 1번 서버가 구동중이면 2번 서버에 배포 후 로드밸런서(리버스 프록시) set 전환

배치의 경우 무중단 배포를 위해 방법이 크게 2가지가 있다

- 구동중인 Batch Application 다 끝나면 배포되도록 계속 확인 (운이 없게 다른 Job이 실행되면 영원히 배포되지 않음)
- readlink를 활용 (더 좋은 방법)

### realink 배포

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd59b895-b84c-4e58-bb6d-68f5f78ef946/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bd59b895-b84c-4e58-bb6d-68f5f78ef946/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/82690316-7f8c-428c-904a-931c4050b945/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/82690316-7f8c-428c-904a-931c4050b945/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c1f4824-cd23-48b4-8bb5-6d38ef7ab2a8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c1f4824-cd23-48b4-8bb5-6d38ef7ab2a8/Untitled.png)

## 파이프라인

Step을 여러개로 나누기보다는 Job 1개에 Step 1개로 배치하는것이 좋다. 추후에 융통성있게 제외하거나 추가하는 것이 쉬워진다.

## 멱등성

- 연산을 여러번 적용하더라도 결과가 달라지지 않는 성질

애플리케이션 개발에서 멱등성이 깨지는 경우

- 제어할 수 없는 코드를 직접 생성할때

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f12a4ec5-4a4f-444d-82b2-aa0d9930283b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f12a4ec5-4a4f-444d-82b2-aa0d9930283b/Untitled.png)

파라미터가 없는 형태에서 파라미터를 외부에서 넣어줄 수 있도록 JobParameter로 변경

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c491b140-a5b1-45eb-aed0-74745eeb6dde/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c491b140-a5b1-45eb-aed0-74745eeb6dde/Untitled.png)

젠킨스의 Date Parameter 플러그인을 추가해서 날짜형식에 맞추어서 파라미터 제공

---

## 테스트 코드

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d8b4f97-8763-4fb5-8051-ae96856eeb80/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d8b4f97-8763-4fb5-8051-ae96856eeb80/Untitled.png)

하지만 Spring Batch 테스트 케이스가 많은 경우 문제가 된다.

→ 테스트 속도가 기하 급수적으로 느려진다.

이유는 @ConditionalOnProperty!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a22363e6-58dc-4e61-be57-d9240c6a3925/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a22363e6-58dc-4e61-be57-d9240c6a3925/Untitled.png)

해결하기 위해 우선 Environment가 변경되는 조건을 확인

- 테스트 코드에서 @MockBean / @SpyBean 사용할때
- 테스트 코드에서 @TestPropertySource로 환경변수 변경할 때
- @ConditionalOnProperty 로 테스트마다 Config가 다를 때

해결방법

→ @ConditionalOnProperty, @TestPropertySource 제거!

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c93fd05-b5a7-46e7-8afa-f5aa7383cc58/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c93fd05-b5a7-46e7-8afa-f5aa7383cc58/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70032d33-691e-4e35-8de1-b8ce466fbf91/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70032d33-691e-4e35-8de1-b8ce466fbf91/Untitled.png)

- Context 1개만 가지고 모든 테스트 해결

---

## JPA & Spring Batch

JPA N+1 문제

@OneToMany 관계에서 하위 엔티티들을 가져올때마다 조회 쿼리가 추가로 발생하는 이슈

→ Join Fetch 로 해결 

하지만 하위 엔티티 2개 종류 이상에서 Join Fetch 사용시 MultipleBagFetchException 발생

→ default_batch_fetch_size

하위 엔티티를 Loading할때 지정된 숫자만큼 상위 엔티티 Id를 Where In ()에 넣어서 조회

→ default_batch_fetch_size: 1000 이면 10000건의 하위는 부모 1 + 자식 10 총 11건으로 해결

하지만 이 옵션은 JpaPagingItemReader에서는 작동하지 않는다.

(HibernateCursorItemReader는 가능) → JpaRepository에서도 사용할 수 있는 정상 기능

## Persist Writer

JPA Persist Writer

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72b121f6-9c57-44c1-af9d-085b10a6a10b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72b121f6-9c57-44c1-af9d-085b10a6a10b/Untitled.png)

처음 데이터가 save 될 때도 Update 쿼리가 항상  실행된다..

→ entityManager.persist(item)으로 변경

모든 Item에 대해 Persist로 제공 (Spring Batch 4.2 에서 기본 옵션으로 제공)
