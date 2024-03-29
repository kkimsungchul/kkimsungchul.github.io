---
layout: post
title: "[Spring Boot] JPA 강의 정리-38 (엔티티 직접 사용)"
subtitle: "Spring Boot JPA 강의 정리-38 (엔티티 직접 사용)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-38(엔티티 직접 사용)  
  
## 엔티티 직접 사용 - 기본키 값  
	- JPQL에서 엔티티를 직접 사용하면 SQL에서 해당 엔티티의 기본 키 값을 사용  
  
	ex1)  
  
	JPQL  
	=====================================================================  
	select count(m.id) from Member m // 엔티티의 아이디를 사용  
	select count(m) from Member m //엔티티를 직접 사용  
	=====================================================================  
  
	SQL ( JPQL 둘다 아래의 쿼리 사용)  
	=====================================================================  
	select count(m.id) as cnt from Member m  
	=====================================================================  
  
	ex2)  
  
	JQPL  
	=====================================================================  
	//엔티티 직접 사용  
	String jpql1 = "select m from Member m where m = :member";  
	List<Member> resultList1 = em.createQuery(jpql1)  
			.setParameter("member", member)  
			.getResultList();  
  
	//식별자를 직접 전달  
	long memberId = 1;  
	String jpql2 = "select m from Member m where m.id = :memberId";  
	List<Member> resultList2 = em.createQuery(jpql2)  
			.setParameter("memberId", memberId)  
			.getResultList();  
  
	=====================================================================  
  
	SQL - 아래의 쿼리문을보면 두개가 똑같음  
	=====================================================================  
	Hibernate: //엔티티 직접 사용  
		/* select  
			m  
		from  
			Member m  
		where  
			m = :member */ select			//여기서부터 보면 됨  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id5_0_,  
				member0_.type as type3_0_,  
				member0_.username as username4_0_  
			from  
				Member member0_  
			where  
				member0_.id=?  
  
	Hibernate: //식별자를 직접 전달  
		/* select  
			m  
		from  
			Member m  
		where  
			m.id = :memberId */ select		//여기서부터 보면 됨  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id5_0_,  
				member0_.type as type3_0_,  
				member0_.username as username4_0_  
			from  
				Member member0_  
			where  
				member0_.id=?  
  
	=====================================================================  
  
## 엔티티 직접 사용 - 외래 키 값  
  
	JPQL  
	=====================================================================  
	Team team1 = em.find(Team.class,1L);  
	//엔티티 직접 사용  
	String jpql3 = "select m from Member m  where m.team = :team";  
	List<Member> resultList3 = em.createQuery(jpql3)  
			.setParameter("team", team1)  
			.getResultList();  
  
	//식별자를 직접 전달  
	String jpql4 = "select m from Member m  where m.team.id = :teamId";  
	List<Member> resultList4 = em.createQuery(jpql4)  
			.setParameter("teamId", team1.getId())  
			.getResultList();  
  
	=====================================================================  
  
	SQL  
	=====================================================================  
	Hibernate:					//엔티티 직접 사용  
		/* select  
			m  
		from  
			Member m  
		where  
			m.team = :team */ select			//여기서부터 확인  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id5_0_,  
				member0_.type as type3_0_,  
				member0_.username as username4_0_  
			from  
				Member member0_  
			where  
				member0_.TEAM_ID=?  
	Hibernate:					//식별자를 직접 전달  
		/* select  
			m  
		from  
			Member m  
		where  
			m.team.id = :teamId */ select		//여기서부터 확인  
				member0_.id as id1_0_,  
				member0_.age as age2_0_,  
				member0_.TEAM_ID as team_id5_0_,  
				member0_.type as type3_0_,  
				member0_.username as username4_0_  
			from  
				Member member0_  
			where  
				member0_.TEAM_ID=?  
	7월 09, 2023 3:36:09 오후 org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl$PoolState stop  
	INFO: HHH10001008: Cleaning up connection pool [jdbc:h2:tcp://localhost/~/test]  
  
	Process finished with exit code 0  
  
	=====================================================================  

{% endraw %}
