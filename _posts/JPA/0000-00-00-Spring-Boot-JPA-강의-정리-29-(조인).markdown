---
layout: post
title: "[Spring Boot] JPA 강의 정리-29 (조인)"
subtitle: "Spring Boot JPA 강의 정리-29 (조인)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-29(조인)  
  
## 내부 조인  
	SELECT m FROM Member m [INNER] JOIN m.team t  
  
	- 실행 코드  
	=====================================================================  
	String query = "select m from Member m inner join m.team t";  
	List<Member> result = em.createQuery(query , Member.class)  
					.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			m  
		from  
			Member m  
		inner join  
			m.team t */ select  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id4_0_,  
				member0_.username as username3_0_  
			from  
				Member member0_  
			inner join  
				Team team1_  
					on member0_.TEAM_ID=team1_.id  
  
	=====================================================================  
  
## 외부 조인  
	SELECT m FROM Member m LEFT [OUTER] JOIN m.team t  
  
	- 실행 코드  
	=====================================================================  
	String query2 = "select m from Member m left join m.team t";  
	List<Member> result2 = em.createQuery(query2 , Member.class)  
			.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			m  
		from  
			Member m  
		left outer join  
			m.team t */ select  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id4_0_,  
				member0_.username as username3_0_  
			from  
				Member member0_  
			left outer join  
				Team team1_  
					on member0_.TEAM_ID=team1_.id  
  
	=====================================================================  
  
## 세타 조인  
	SELECT count(m) from Member, Team t where m.username = t.name  
  
	- 실행 코드  
	=====================================================================  
	String query3 = "select m from Member m , Team t where m.username = t.name";  
	List<Member> result3 = em.createQuery(query3 , Member.class)  
			.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			m  
		from  
			Member m ,  
			Team t  
		where  
			m.username = t.name */ select  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id4_0_,  
				member0_.username as username3_0_  
			from  
				Member member0_ cross  
			join  
				Team team1_  
			where  
				member0_.username=team1_.name  
  
	=====================================================================  
  
## 조인 - ON 절  
	- JPA 2.1 버전 부터 ON 절을 활용한 조인을 지원함  
	- 조인 대상 필터링 지원  
	- 연관관계 없는 엔티티 외부 조인 (하이버네이트 5.1부터)  
  
	- 실행 코드  
	=====================================================================  
	String query3 = "SELECT m  FROM Member m LEFT JOIN Team t on m.username = t.name";  
	List<Member> result3 = em.createQuery(query3 , Member.class)  
			.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* SELECT  
			m  
		FROM  
			Member m  
		LEFT JOIN  
			Team t  
				on m.username = t.name */ select  
					member0_.id as id1_0_,  
					member0_.age as age2_0_,  
					member0_.TEAM_ID as team_id4_0_,  
					member0_.username as username3_0_  
			from  
				Member member0_  
			left outer join  
				Team team1_  
					on (  
						member0_.username=team1_.name  
					)  
	=====================================================================  

{% endraw %}
