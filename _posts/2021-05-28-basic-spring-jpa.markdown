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
