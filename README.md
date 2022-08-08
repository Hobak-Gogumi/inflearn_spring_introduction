# inflearn_spring_introduction
인프런 김영한 강사님의 [스프링 입문 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)를 들으면서 기록한 리포지토리 입니다.

개발환경

자바 11

H2 Database 1.4.200 버전

강의 목표: 예제를 만들어가면서 스프링 개발의 전반적인 감을 잡고 큰 그림을 머리속에 그리는 것
***
## 🍀 스프링 웹 개발 기초
스프링 웹 개발에는 크게 3가지 방법이 있다.
+ 정적 컨텐츠
    - ex) Welcome Page
    - 파일을 그대로 내려줌
    - static 폴더에 위치
+ MVC와 템플릿 엔진
    - 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(view resolver)가 화면을 찾아서 처리한다.
    - `resources:templates/ + {반환 문자 값} + .html` 파일을 찾음
+ API
    - `@ResponseBody`를 사용하면 뷰 리졸버를 사용하지 않고 HTTP의 바디부분에 문자 내용을 직접 반환
    - 객체를 반환하면 JSON으로 변환됨
    - `viewResolver` 대신 `HttpMessageConverter`가 동작

## 🍀 백엔드 개발
+ 일반적인 웹 어플리케이션 계층 구조
    - ![웹 어플리케이션 구조](https://github.com/Hobak-Gogumi/inflearn_spring_introduction/blob/main/img/structure.PNG)
+ 테스트 케이스 작성
    - `src/test/java` 하위 폴더에 생성

## 🍀 스프링 빈과 의존관계
스프링 빈을 등록하는 방법은 2가지가 있다.
+ 컴포넌트 스캔과 자동 의존관계 설정
    - `@Component` 혹은 `@Controller`, `@Service`, `@Repository` 애노테이션이 있으면 스프링 빈으로 자동 등록
    - 생성자에 `@Autowired` 애노테이션을 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입
+ 자바 코드로 직접 스프링 빈 등록하기
    - 설정 클래스 파일을 따로 만들고 `@Configuration`, `@Bean` 애노테이션 사용
    - 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 할 경우 설정을 통해 스프링 빈으로 등록

## 🍀 스프링 DB 접근 기술
+ 순수 JDBC
    - 반복코드 많음
    - SQL 직접 작성
+ 스프링 JdbcTemplate
    - JDBC 에서 본 반복코드 제거
    - SQL 직접 작성
+ JPA
    - 반복코드 제거
    - 기본적인 SQL도 JPA가 직접 만들어서 실행
    - `@Entity` 애노테이션을 붙이면 JPA가 관리하는 엔티티
    - `@Transactional` 해당 클레스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋
    - **JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행**
+ 스프링 데이터 JPA
    - JpaRepository를 상속받는 인터페이스를 작성
    - 스프링 빈에 알아서 등록해줌
+ 스프링 통합 테스트
    - 스프링 컨테이너와 DB까지 연결한 테스트
    - `@SpringBootTest` 스프링 컨테이너와 테스트를 함께 실행
    - 테스트 케이스에 `@Transactional` 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 

## 🍀 AOP
+ AOP: Aspect Oriented Programming (관점 지향 프로그래밍)
    - `공통 관심 사항(cross-cutting concern)`과 `핵심 관심 사항(core concern)` 분리
    - 공통 관심 사항 모듈에 `@Aspect` 애노테이션 적용
    - `@Around`로 범위 지정
    - 스프링 빈 등록(컴포넌트 스캔, 등록 방식 둘다 가능)