---
layout: post
title: "[Spring Boot] JPA 강의 정리-31 (JQPL 타입 표현과 기타식)"
subtitle: "Spring Boot JPA 강의 정리-31 (JQPL 타입 표현과 기타식)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-31(JQPL 타입 표현과 기타식)  
  
## JPQL 타입 표현  
	- 문자 : 'HELLO' , She''s'  
	- 숫자 : 10L(Long) , 10D(Double) , 10F(Float)  
	- Boolean : TRUE , FALSE  
	- ENUM : jpabook.MemberType.Admin(패키지명 포함)  
	- Entity Type : TYPE(m) = Member (상속 관계에서 사용)  
  
## JPQL 기타  
	SQL과 문법이 같음  
	- EXISTS , IN  
	- AND , OR , NOT  
	- = , > , >= , < , <= , <>  
	- BETWEEN , LIKE , IS NULL  
  
## Enum 예제  
	※ 테스트 작성시 Domain 에 @Enumerated(EnumType.STRING) 어노테이션 추가  
	=====================================================================  
	public class Member {  
		...중략...  
		@Enumerated(EnumType.STRING)  
		private MemberType type;  
	}  
	=====================================================================  
  
	- 실행 코드  
	=====================================================================  
	Member member = new Member();  
	member.setUsername("김성철");  
	member.setAge(20);  
	member.setType(MemberType.ADMIN);  
	em.persist(member);  
	em.flush();  
  
	String query = "select m.username , 'HELLO' , TRUE From Member m where m.type = domain.MemberType.ADMIN";  
	List<Object[]> result = em.createQuery(query)  
			.getResultList();  
	=====================================================================  
  
	- 실행코드 (파라메터 바인딩)  
	=====================================================================  
	String query = "select m.username , 'HELLO' , TRUE From Member m where m.type = :userType";  
	List<Object[]> result = em.createQuery(query)  
			.setParameter("userType" , MemberType.ADMIN)  
			.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			m.username ,  
			'HELLO' ,  
			TRUE  
		From  
			Member m  
		where  
			m.type = domain.MemberType.ADMIN */ select  
				member0_.username as col_0_0_,  
				'HELLO' as col_1_0_,  
				true as col_2_0_  
			from  
				Member member0_  
			where  
				member0_.type='ADMIN'  
  
	=====================================================================  
  

{% endraw %}
