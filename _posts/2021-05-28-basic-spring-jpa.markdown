---
layout: post
title:  "실전! 스프링 데이터 JPA"
date:   2021-05-28 09:03:36 +0530
categories:
---
---

인프런 - 실전! 스프링 데이터 JPA

---

## [1강] 프로젝트 환경설정

#### - **프로젝트 생성**

  - 스프링 부트 스타터 (https://start.spring.io/)
  - 사용 기능 : web, jpa, h2, lombok
      - SpringBootVersion: 2.2.1
      - groupId: study
      - artifactid: data-jpa

          * 스프링 부트 스타터
          <img data-action="zoom" src='{{ "/image/123.PNG" | relative_url }}' alt='absolute'>

          * bulid.gradle 열기
          <img data-action="zoom" src='{{ "/image/124.PNG" | relative_url }}' alt='absolute'>

          * 실행 확인
          <img data-action="zoom" src='{{ "/image/125.PNG" | relative_url }}' alt='absolute'>

          * 컨트롤러 생성 후 실행
          <img data-action="zoom" src='{{ "/image/126.PNG" | relative_url }}' alt='absolute'>

          * 설정 변경
          <img data-action="zoom" src='{{ "/image/127.PNG" | relative_url }}' alt='absolute'>

          * lombok 설정
          <img data-action="zoom" src='{{ "/image/128.PNG" | relative_url }}' alt='absolute'>

#### - **라이브러리 살펴보기**

  - 인텔리제이 터미널 설정

    <img data-action="zoom" src='{{ "/image/129.PNG" | relative_url }}' alt='absolute'>

  - 스프링 부트 라이브러리
      * spring-boot-starter-web
          - spring-boot-starter-tomcat: 톰캣 (웹서버)
          - spring-webmvc: 스프링 웹 MVC

      * spring-boot-starter-data-jpa      
          - spring-boot-starter-aop
          - spring-boot-starter-jdbc
              - HikariCP 커넥션 풀 (부트 2.0 기본)
          - hibernate + JPA: 하이버네이트 + JPA
          - spring-data-jpa: 스프링 데이터 JPA

      * spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
          - spring-boot
              - spring-core
          - spring-boot-starter-logging
              - logback, slf4j  


    - 테스트 라이브러리
        - spring-boot-starter-test
            - junit: 테스트 프레임워크, 스프링 부트 2.2부터 junit5( jupiter ) 사용
                - 과거 버전은 vintage
            - assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
            - spring-test: 스프링 통합 테스트 지원


    - 핵심 라이브러리
        - 스프링 MVC
        - 스프링 ORM
        - JPA, 하이버네이트
        - 스프링 데이터 JPA

    - 기타 라이브러리
        - H2 데이터베이스 클라이언트
        - 커넥션 풀: 부트 기본은 HikariCP
        - 로깅 SLF4J & LogBack
        - 테스트

#### - **H2 데이터베이스 설치**

  - https://www.h2database.com
  - h2 데이터베이스 버전은 스프링 부트 버전에 맞춤
  - 데이터베이스 파일 생성 방법
      - jdbc:h2:~/datajpa (최소 한번)
      - ~/datajpa.mv.db 파일 생성 확인
      - 이후 부터는 jdbc:h2:tcp://localhost/~/datajpa 이렇게 접속

        <img data-action="zoom" src='{{ "/image/130.PNG" | relative_url }}' alt='absolute'>

#### - **스프링 데이터 JPA와 DB 설정, 동작확인**

  ```c
  # main/resources/application.yml (properties 삭제)
  spring:
   datasource:
    url: jdbc:h2:tcp://localhost/~/datajpa
    username: sa
    password:
    driver-class-name: org.h2.Driver

   jpa:
    hibernate:
      ddl-auto: create # 로딩시점에 테이블을 전부 drop 후 재생성, 운영 시 사용 X
      properties:
        hibernate:
          # show_sql: true # jpa 실행 쿼리를 콘솔에 조회
          format_sql: true

  logging.level:
   org.hibernate.SQL: debug # 로그로 남기기
  # org.hibernate.type: trace # 파라미터까지 볼 수 있는 옵션
  ```    

1. Entity 생성

    <img data-action="zoom" src='{{ "/image/131.PNG" | relative_url }}' alt='absolute'>  
    @GeneratedValue  
    PK 자동 생성

2. JPA Repository 생성

    <img data-action="zoom" src='{{ "/image/132.PNG" | relative_url }}' alt='absolute'>  
    @PersistenceContext  
    스프링 컨테이너가 Entity Manager를 가져다줌

3. 테스트 - JPA Repository

    *  ctrl + shift + T

          <img data-action="zoom" src='{{ "/image/133.PNG" | relative_url }}' alt='absolute'>   

    *  생성자 추가

          <img data-action="zoom" src='{{ "/image/134.PNG" | relative_url }}' alt='absolute'>

          <img data-action="zoom" src='{{ "/image/135.PNG" | relative_url }}' alt='absolute'>  
          기본 생성자 protected 사용 이유  
          jpa가 프록시를 사용할 때 쓸 수 있도록 private -> protected 로 변경

    *  실행 시 , 에러 발생

          <img data-action="zoom" src='{{ "/image/137.PNG" | relative_url }}' alt='absolute'>  

    *  @Transactional 어노테이션 추가하여 해결

          <img data-action="zoom" src='{{ "/image/138.PNG" | relative_url }}' alt='absolute'>

    * assertThat(findMember).isEqualTo(member); 테스트시 결과는?
         - true  
           한 트랜잭션 내에서는 영속성 컨텍스트 보장

4. Spring Data Repository 생성

    <img data-action="zoom" src='{{ "/image/139.PNG" | relative_url }}' alt='absolute'>  

5. 테스트 - Spring Data Repository

    *  실행 결과 ( save, findById 기본 내장 )

          <img data-action="zoom" src='{{ "/image/140.PNG" | relative_url }}' alt='absolute'>  

6. 쿼리 파라미터 로그 남기기

    * build.gradle / dependencies 에  
      implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.7' 추가  
      운영에서는 성능저하로 인해 사용 고려

        <img data-action="zoom" src='{{ "/image/141.PNG" | relative_url }}' alt='absolute'>

## [2강] 예제 도메인 모델

#### - **예제 도메인 모델과 동작확인**

<img data-action="zoom" src='{{ "/image/142.PNG" | relative_url }}' alt='absolute'>

1. Entity 생성

      <img data-action="zoom" src='{{ "/image/143.PNG" | relative_url }}' alt='absolute'>  

      <img data-action="zoom" src='{{ "/image/144.PNG" | relative_url }}' alt='absolute'>      


      * Jpa 엔티티 기본생성자는  
      @NoArgsConstructor(access = AccessLevel.PROTECTED) 로 대체 가능하다.

      * Member Entity
          1. changeTeam 함수 추가
              ```c
              public void changeTeam(Team team) {
                  this.team = team;
                  team.getMembers().add(this);
              }
              ```

          2. 지연로딩으로 변경
              ```c
              @ManyToOne(fetch = FetchType.LAZY)
              @JoinColumn(name = "team_id")
              private Team team;
              ```

      * Team Entity
          1. 생성자 추가
                ```c
                public Team(String name) {
                    this.name = name;
                }
                ```         
2. 테스트

      <img data-action="zoom" src='{{ "/image/145.PNG" | relative_url }}' alt='absolute'>  

## [3강] 공통 인터페이스 기능

#### - **순수 JPA 기반 리포지토리 만들기**

  - MemberjpaRepository.java

      * delete 함수 추가  

          <img data-action="zoom" src='{{ "/image/146.PNG" | relative_url }}' alt='absolute'>  

      * findAll 함수 추가    
          - jpa 기반 리포지토리에서는 findall 조회 시, JPQL을 사용

            <img data-action="zoom" src='{{ "/image/147.PNG" | relative_url }}' alt='absolute'>   

      * Optional find 함수 추가

          <img data-action="zoom" src='{{ "/image/148.PNG" | relative_url }}' alt='absolute'>   

      * count 함수 추가

          <img data-action="zoom" src='{{ "/image/149.PNG" | relative_url }}' alt='absolute'>   

  - TeamRepository.java

      <img data-action="zoom" src='{{ "/image/150.PNG" | relative_url }}' alt='absolute'>

  - JPA의 자동 변경 감지기능으로 인해, update 함수가 필요 없음

  - 테스트
      * MemberjpaRepository.java

        <img data-action="zoom" src='{{ "/image/151.PNG" | relative_url }}' alt='absolute'>

        <img data-action="zoom" src='{{ "/image/152.PNG" | relative_url }}' alt='absolute'>

  - <span style="color:red">반복적인 CRUD가 발생함</span>

#### - **공통 인터페이스 설정**

  - java config 설정
      * 스프링 부트 사용 시, 생략 가능
      * 만약 위치가 달리진다면 @EnableJpaRepositories 필요

        <img data-action="zoom" src='{{ "/image/153.PNG" | relative_url }}' alt='absolute'>      

  - Spring data jpa가 인터페이스를 보고 구현 클래스를 만들어서 Injection 받아 사용
      * 실제 출력 시 : memberRepository.getClass() class com.sun.proxy.$ProxyXXX
      * @Repository 애노테이션 생략 가능

        <img data-action="zoom" src='{{ "/image/154.PNG" | relative_url }}' alt='absolute'>   

#### - **공통 인터페이스 적용**

  ```c
  public interface TeamRepository extends JpaRepository<ENTITY, PKTYPE> {
  }
  ```

  - 테스트

      <img data-action="zoom" src='{{ "/image/155.PNG" | relative_url }}' alt='absolute'>

#### - **공통 인터페이스 분석**

  <img data-action="zoom" src='{{ "/image/156.PNG" | relative_url }}' alt='absolute'>

- 주요 메서드
    * save(S) : 새로운 엔티티는 저장하고 이미 있는 엔티티는 병합한다.
    * delete(T) : 엔티티 하나를 삭제한다. 내부에서 EntityManager.remove() 호출
    * findById(ID) : 엔티티 하나를 조회한다. 내부에서 EntityManager.find() 호출
    * getOne(ID) : 엔티티를 프록시로 조회한다. 내부에서 EntityManager.getReference() 호출
    * findAll(…) : 모든 엔티티를 조회한다.  
                   정렬( Sort )이나 페이징( Pageable ) 조건을 파라미터로 제공할 수 있다.

## [4강] 쿼리 메소드 기능

#### - **메소드 이름으로 쿼리 생성**

  - 도메인에 특화된 조회 기능을 공통 기능을 사용하지 않고 어떻게 해결할까?  
    -> 쿼리 메소드 기능

  - 메소드 이름으로 쿼리 생성
      * 참고 https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods  

      * 순수 Jpa (직접 쿼리를 작성해야 함)
        ```c
        public List<Member> findByUsernameAndAgeGreaterThen(String username, int age) {
            return em.createQuery("select m from Member m where m.username = :username and m.age > :age")
                    .setParameter("username", username)
                    .setParameter("age",age)
                    .getResultList();
        }
        ```

        <img data-action="zoom" src='{{ "/image/157.PNG" | relative_url }}' alt='absolute'>

      * 스프링 데이터 Jpa
      * <span style="color:red">애플리케이션 로딩시점에 오류 제어 가능</span>

        ```c
        List<Member> findByUsernameAndAgeGreaterThan(String username, int age);
        ```

        <img data-action="zoom" src='{{ "/image/158.PNG" | relative_url }}' alt='absolute'>

#### - **JPA NamedQuery**

  - 실무에서 자주 쓰이지 않음
  - Entity에 선언

    <img data-action="zoom" src='{{ "/image/159.PNG" | relative_url }}' alt='absolute'>

  - 순수 Jpa에서의 사용

    <img data-action="zoom" src='{{ "/image/160.PNG" | relative_url }}' alt='absolute'>

  - 스프링 데이터 Jpa 에서의 사용  
    * jpql로 작성된 네임드 쿼리 사용시 @Param 으로 변수를 지정

      <img data-action="zoom" src='{{ "/image/161.PNG" | relative_url }}' alt='absolute'>

  - <span style="color:red">애플리케이션 로딩시점에 오류 제어 가능</span>

#### - **@Query, 리포지토리 메소드에 쿼리 정의하기**

  - 실무에서 자주 사용하는 방법

    <img data-action="zoom" src='{{ "/image/162.PNG" | relative_url }}' alt='absolute'>

  - 테스트

    <img data-action="zoom" src='{{ "/image/163.PNG" | relative_url }}' alt='absolute'>

  - <span style="color:red">애플리케이션 로딩시점에 오류 제어 가능</span>

#### - **@Query, 값, DTO 조회하기**

  - 단순한 값 하나를 조회

    <img data-action="zoom" src='{{ "/image/164.PNG" | relative_url }}' alt='absolute'>

    <img data-action="zoom" src='{{ "/image/165.PNG" | relative_url }}' alt='absolute'>

  - DTO로 직접 조회
      * dto 생성  

        <img data-action="zoom" src='{{ "/image/166.PNG" | relative_url }}' alt='absolute'>

      * 리포지토리 구현  
        <span style="color:red">★ @Query 사용 시, Select 산하 부분을 new 패키지명(조회 컬럼) 으로 적어줘야 함</span>

          <img data-action="zoom" src='{{ "/image/167.PNG" | relative_url }}' alt='absolute'>

      * 테스트

          <img data-action="zoom" src='{{ "/image/168.PNG" | relative_url }}' alt='absolute'>

#### - **파라미터 바인딩**

  - 가급적 이름기반을 사용하자 !

  - IN 절 사용하기

    <img data-action="zoom" src='{{ "/image/169.PNG" | relative_url }}' alt='absolute'>

  - 테스트

    <img data-action="zoom" src='{{ "/image/170.PNG" | relative_url }}' alt='absolute'>
