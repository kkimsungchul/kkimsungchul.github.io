---
layout: post
title: "[Spring Boot] JPA 강의 정리-25 (객체지향 쿼리 언어)"
subtitle: "Spring Boot JPA 강의 정리-25 (객체지향 쿼리 언어)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-25(객체지향 쿼리 언어)  
  
## JPQL  
	- 가장 단순한 조회 방법  
		EntityManager.find()  
		객체 그래프 탐색( a.getB().getC() )  
	- JPA를 사용하면 엔티티 객체를 중심으로 개발  
	- 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색  
	- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능  
	- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요  
  
	- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공  
	- SQL과 문법 유사, SELECT , FROM , WHERE , GROUP BY , HAVING , JOIN 지원(ANSI 표준)  
	- JPQL은 엔티티 객체를 대상으로 쿼리  
	- SQL은 데이터베이스 테이블 대상으로 쿼리  
  
	- 테이블이 아닌 객체를 대상으로 검색하는 객체 지향 쿼리  
	- SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않음  
	- JPQL을 정의하면 "객체지향 SQL" 이라고보면 됨  
  
	- 동적 쿼리를 만들기 어려움  
  
## JPQL 예시  
	- member가 이름에 포함된 사용자 검색 예시  
	=====================================================================  
	List<Member> resultList =  em.createQuery(  
			"select m from Member m where m.username like '%member%'", Member.class  
	).getResultList();  
	=====================================================================  
  
	em.createQuery() 안에 쿼리를 작성하면 되며, 리턴될 객체 타입을 두번째 매개변수로 주면 됨  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			m  
		from  
			Member m  
		where  
			m.username like '%member%' */ select  
				member0_.MEMBER_ID as member_i1_6_,  
				member0_.createdBy as createdb2_6_,  
				member0_.createdDate as createdd3_6_,  
				member0_.lastModifiedBy as lastmodi4_6_,  
				member0_.lastModifiedDate as lastmodi5_6_,  
				member0_.city as city6_6_,  
				member0_.street as street7_6_,  
				member0_.zipcode as zipcode8_6_,  
				member0_.TEAM_ID as team_id15_6_,  
				member0_.USERNAME as username9_6_,  
				member0_.WORK_CITY as work_ci10_6_,  
				member0_.WORK_STREET as work_st11_6_,  
				member0_.WORK_ZIPCODE as work_zi12_6_,  
				member0_.endDate as enddate13_6_,  
				member0_.startDate as startda14_6_  
			from  
				Member member0_  
			where  
				member0_.USERNAME like '%member%'  
  
	=====================================================================  
  
## JPA Criteria  
	- 자바 표준 스펙에서 제공해주는 기능  
	- 문자가 아닌 자바코드로 JPQL을 작성할 수 있음  
	- JPQL 빌더 역할  
	- 오타에 대한 컴파일 오류를 확인 가능  
	- 동적 쿼리를 작성하기 좋음  
  
	- 다만SQL스럽지 않음  
	- 실무에서 사용하기 힘듬 (유지보수가 어려움)  
	- Criteria 대신에 QueryDSL 사용 권장  
  
## JPA Criteria 예시  
  
	=====================================================================  
	CriteriaBuilder cb = em.getCriteriaBuilder();  
	CriteriaQuery<Member> query =  cb.createQuery(Member.class);  
	Root<Member> m = query.from(Member.class);  
	CriteriaQuery<Member> cq = query.select(m).where(cb.equal(m.get("username"), "member"));  
	List<Member> resultList2 = em.createQuery(cq).getResultList();  
	System.out.println(resultList2);  
	=====================================================================  
  
	-실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			generatedAlias0  
		from  
			Member as generatedAlias0  
		where  
			generatedAlias0.username=:param0 */ select  
				member0_.MEMBER_ID as member_i1_6_,  
				member0_.createdBy as createdb2_6_,  
				member0_.createdDate as createdd3_6_,  
				member0_.lastModifiedBy as lastmodi4_6_,  
				member0_.lastModifiedDate as lastmodi5_6_,  
				member0_.city as city6_6_,  
				member0_.street as street7_6_,  
				member0_.zipcode as zipcode8_6_,  
				member0_.TEAM_ID as team_id15_6_,  
				member0_.USERNAME as username9_6_,  
				member0_.WORK_CITY as work_ci10_6_,  
				member0_.WORK_STREET as work_st11_6_,  
				member0_.WORK_ZIPCODE as work_zi12_6_,  
				member0_.endDate as enddate13_6_,  
				member0_.startDate as startda14_6_  
			from  
				Member member0_  
			where  
				member0_.USERNAME=?  
  
	=====================================================================  
## QueryDSL  
	http://querydsl.com/  
	http://querydsl.com/static/querydsl/5.0.0/reference/html_single/  
	- 문자가 아닌 자바코드로 JPQL을 작성할 수 있음  
	- JPQL 빌더 역할  
	- 컴파일 시점에 문법 오류를 찾을 수 있음  
	- 동적 쿼리 작성 편리함  
	- 단순하고 쉬움  
	- 실무 사용 권장  
	- JPQL을 알아야 사용하는데 지장이 없음  
  
## 네이티브 SQL  
	- JPQ가 제공하는 SQL을 직접 사용하는 기능  
	- JPQL로 해결할 수 없는 특정 데이터베이스에 의존적인 기능  
	ex) 오라클 CONNECT BY , 특정DB만 사용하는 SQL 힌트  
  
## 네이티브 SQL 예시  
	=====================================================================  
	//NativeQuery  
	List<Member> resultList3 = em.createNativeQuery("select * from member",Member.class)  
					.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* dynamic native SQL query */ select  
			*  
		from  
			member  
	=====================================================================  
  
## JDBC API 직접 사용 , MyBatis , SpringJdbcTemplate  
	- JPA를 사용하면서 JDBC 커넥션을 직접 사용하거나, 스프링 JdbcTemplate , MyBatis등을 함꼐 사용 가능  
	- 단 영속성 컨텍스트를 적절한 시점에 강제로 플러시 필요 (플러시를 해야 DB에 데이터가 존재함)  
	- flush 는 commit , query 가 실행될 때 실행됨  
  
	ex) JPA를 우회해서 SQL을 실행하기 직전에 영속성 컨텍스트 수동 플러시  
  

{% endraw %}
