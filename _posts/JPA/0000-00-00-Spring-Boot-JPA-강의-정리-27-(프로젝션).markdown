---
layout: post
title: "[Spring Boot] JPA 강의 정리-27 (프로젝션)"
subtitle: "Spring Boot JPA 강의 정리-27 (프로젝션)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-27(프로젝션)  
  
## 프로젝션  
	- SELECT 절에 조회할 대상을 지정하는 것  
	- 프로젝션 대상 : 엔티티 , 임베디드 타입 , 스칼라 타입 (숫자 문자등 기본 데이터 타입)  
	ex)  
		- SELECT m FROM Member m -> 엔티티 프로젝션  
		- SELECT m.team FROM Member m -> 엔티티 프로젝션  
		- SELECT m.address FROm Member m-> 임베디드 타입 프로젝션  
		- SELECT m.username , m.age FROM Member m -> 스칼라 타입 프로젝션  
		- DISTINCT로 중복 제거  
  
## 엔티티 프로젝션  
	- 영속성 컨텍스트에서 관리가 됨  
  
	- 엔티티 프로젝션은 아래와 같이 쿼리문을 날려도 알아서 JOIN을 해서 값을 가져옴  
		※ 다만 아래와같이 하면 튜닝이나 수정시 어려움이 발생할 수 있으므로 왠만하면 실제 실행할 쿼리와 비슷하게 작성  
		- 실행 코드  
		=====================================================================  
		List<Team> result2 = em.createQuery("select m.team from Member m" , Team.class)  
				.getResultList();  
		=====================================================================  
		- 실행 쿼리  
		=====================================================================  
		Hibernate:  
			/* select  
				m.team  
			from  
				Member m */ select  
					team1_.id as id1_3_,  
					team1_.name as name2_3_  
				from  
					Member member0_  
				inner join  
					Team team1_  
						on member0_.TEAM_ID=team1_.id  
		=====================================================================  
  
## 임베디드 타입 프로젝션  
  
	- 실행 코드  
	=====================================================================  
	em.createQuery("select o.address from Order o" , Address.class)  
			.getResultList();  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			o.address  
		from  
  
		Order o */ select  
			order0_.city as col_0_0_,  
			order0_.street as col_0_1_,  
			order0_.zipcode as col_0_2_ from  
				ORDERS order0_  
	=====================================================================  
  
## 스칼라 타입 프로젝션  
	- 실행 코드  
	=====================================================================  
	em.createQuery("select distinct m.username , m.age from Member m")  
			.getResultList();  
	=====================================================================  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			distinct m.username ,  
			m.age  
		from  
			Member m */ select  
				distinct member0_.username as col_0_0_,  
				member0_.age as col_1_0_  
			from  
				Member member0_  
	=====================================================================  
  
## 프로젝션 - 여러 값 조회  
	1. Query 타입으로 조회  
  
		Object타입이여서 Object로 캐스팅을 해줘야 함  
		- 실행 코드  
		=====================================================================  
		List resultList = em.createQuery("select distinct m.username , m.age from Member m")  
				.getResultList();  
		for (Object o : resultList){  
			Object[] objectResult = (Object[])o;  
			System.out.println(objectResult[0]);  
			System.out.println(objectResult[1]);  
		}  
		=====================================================================  
  
		- 실행 쿼리  
		=====================================================================  
		Hibernate:  
			/* select  
				distinct m.username ,  
				m.age  
			from  
				Member m */ select  
					distinct member0_.username as col_0_0_,  
					member0_.age as col_1_0_  
				from  
					Member member0_  
		김성철  
		30  
		=====================================================================  
  
	2. Object[] 타입으로 조회  
		- Object로 캐스팅하지 않기 위해 Object[] 타입으로 넘겨 받음  
		=====================================================================  
		List<Object[]> resultList3 = em.createQuery("select distinct m.username , m.age from Member m")  
							.getResultList();  
					System.out.println(resultList3.get(0)[0]);  
					System.out.println(resultList3.get(0)[1]);  
		=====================================================================  
  
	3. new 명령어로 조회  
		- 단순 값을 DTO로 바로 조회  
		- DTO 클래스의 패키지 명을 포함한 전체 클래스 명 입력  
		- 순서와 타입이 일치하는 생성자 필요  
  
		- 실행 코드  
		=====================================================================  
		 List<MemberDTO> resultList4 = em.createQuery("select new domain.MemberDTO(m.username , m.age) from Member m" , MemberDTO.class)  
							.getResultList();  
					for(MemberDTO memberDTO : resultList4){  
						System.out.println(memberDTO.getUsername());  
						System.out.println(memberDTO.getAge());  
					}  
		=====================================================================  
  
		- 실행 쿼리  
		=====================================================================  
		Hibernate:  
			/* select  
				new domain.MemberDTO(m.username ,  
				m.age)  
			from  
				Member m */ select  
					member0_.username as col_0_0_,  
					member0_.age as col_1_0_  
				from  
					Member member0_  
		김성철  
		30  
		=====================================================================  

{% endraw %}
