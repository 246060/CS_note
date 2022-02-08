# TDD

@Mock vs @MockBean
https://dundung.tistory.com/230




AssertJ 필수 기능 정리 (JAVA)
https://steady-coding.tistory.com/351


AssertJ 주요 기능 공부
https://sun-22.tistory.com/86



rest doc
https://scacap.github.io/spring-auto-restdocs/
https://docs.spring.io/spring-restdocs/docs/2.0.5.RELEASE/reference/html5/
https://tecoble.techcourse.co.kr/post/2020-08-18-spring-rest-docs/
https://techblog.woowahan.com/2597/
https://taetaetae.github.io/2020/03/08/spring-rest-docs-in-spring-boot/



# 양방향 OneToOne 관계에서는 지연로딩이 동작하지 않는다.
출처: https://wave1994.tistory.com/156?category=900343 [훈훈훈]

JPA 구현체인 Hibernate 에서는 양방향 OneToOne 관계에서는 지연로딩이 동작하지 않는다.
정확하게는 테이블을 조회할 때 외래 키를 갖고 있는 테이블(연관 관계의 주인)에서는 지연로딩이 동작하지만, 
mappedBy로 연결된 반대편 테이블은 지연로딩이 동작하지 않고 N + 1 쿼리가 발생한다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcftUJH%2FbtqRqbR1KoW%2FOzlDNexf95hd6Av7y7SDdK%2Fimg.png)

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwnE1e%2FbtqRGqfeqhM%2FuSFxMJAI7mtkgIkjg4kit1%2Fimg.png)

해당 이슈는 JPA의 구현체인 Hibernate 에서 프록시 기능의 한계로 지연 로딩을 지원하지 못하기 때문에 발생한다.

좀 더 자세하게는 프록시 객체를 만들기 위해서는 연관 객체에 값이 있는지 없는지 알아야 한다. 
그런데 Locker Entity 클래스에서 member 값을 알기 위해서는 무조건 Member를 조회해야지 알 수 있다.

따라서 어차피 Member에 대한 쿼리가 발생하기 때문에 hibernate는 프록시 객체를 만들 필요가 없어져 버린다.
결과적으론 지연로딩으로 설정하여도 동작하지 않게 된다.

반대로 Member Entity 클래스를 조회할 때는 Locker에 대한 정보를 가지고 있기 때문에 프록시 객체를 생성 후 지연로딩으로 쿼리를 하게 된다.
이 문제에 대한 대표적인 해결 방법으로는 Fetch Join과 Entity Graph 두 가지가 있다.
사실 두 가지는 명칭만 다를 뿐이지 기능은 똑같다.

Fetch Join과 Entity Graph 를 사용하게 되면 연관된 엔티티도 함께 조회(즉시 로딩)을 할 수 있다.
즉 , 객체 그래프를 쿼리 한 번에 조회하는 개념으로 생각 할 수 있다. 
Fetch Join과 Entity Graph 는 아래 코드처럼 사용할 수 있다.

```java
interface LockerRepository: CrudRepository<Locker, Long> {
    
    /* fetch join example  */
    @Query("select l from Locker l left join fetch l.member where l.id = :lockerId")
    fun findByIdWithFetchJoin(lockerId: Long): Locker

    /* entity graph example */
    @EntityGraph(attributePaths = ["member"])
    fun findTopById(lockerId: Long): Locker
}
``` 

Entity Graph 예제를 보면 쿼리 작성 없이 간단하게 어노테이션을 붙여서 쉽게 사용할 수 있다.
따라서 CrudRepository 인터페이스에서 기본적으로 제공해주는 간단한 쿼리는 @EntityGraph 를 사용할 수 있다.

하지만 쿼리가 복잡하다면 직접 작성을 할 수 밖에 없기 때문에 QueryDSL이나 JPQL을 사용하여 fetch join을 사용할 수 밖에 없다.
마지막으로 Fetch Join과 Entity Graph을 사용하여 Locker Entity 클래스를 조회하면 아래 처럼 쿼리 1개가 발생하는 것을 볼 수 있다.

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbISySu%2FbtqRqQ1bm4e%2FCJ3Yhlh96hJOejHOLJr9b0%2Fimg.png)

