---
layout: post
title: "[Spring Boot] JPA 강의 정리-33 (JPQL 기본함수)"
subtitle: "Spring Boot JPA 강의 정리-33 (JPQL 기본함수)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-33(JPQL 기본 함수)  
  
## CONCAT  
	문자열 합치기  
	- 실행 코드  
	=====================================================================  
	String query = "select concat('a' , 'b') From Member m ";  
  
	List<String> result = em.createQuery(query,String.class)  
			.getResultList();  
	for(String a : result){  
		System.out.println(a);  
	}  
	=====================================================================  
	※ String query = "select 'a' || 'b' From Member m "; 쿼리도 가능  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			concat('a' ,  
			'b')  
		From  
			Member m  */ select  
				('a'||'b') as col_0_0_  
			from  
				Member member0_  
	ab  
	ab  
	=====================================================================  
  
## SUBSTRING  
	문자열 자르기  
	- 실행 코드  
	=====================================================================  
	String query = "select substring(m.username,2,3) From Member m ";  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			substring(m.username,  
			2,  
			3)  
		From  
			Member m  */ select  
				substring(member0_.username,  
				2,  
				3) as col_0_0_  
			from  
				Member member0_  
	성철  
	=====================================================================  
  
## TRIM  
	공백 제거  
  
## LOWER, UPPER  
	대소문자 변경  
  
## LENGTH  
	문자의 길이  
  
## LOCATE  
	위치 반환  
	- 실행 코드  
	=====================================================================  
	String query = "select locate('de', 'abcdefg') From Member m ";  
  
	List<Integer> result = em.createQuery(query,Integer.class)  
			.getResultList();  
	for(Integer a : result){  
		System.out.println(a);  
	}  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			locate('de',  
			'abcdefg')  
		From  
			Member m  */ select  
				locate('de',  
				'abcdefg') as col_0_0_  
			from  
				Member member0_  
	4  
	4  
	=====================================================================  
  
## ABS , SQRT , MOD  
  
## SIZE , INDEX(JPA용도)  
	컬렉션의 사이즈를 되돌려줌  
	- 실행 코드  
	=====================================================================  
	String query = "select size(t.members) From Team t ";  
  
	List<Integer> result = em.createQuery(query,Integer.class)  
			.getResultList();  
	for(Integer a : result){  
		System.out.println(a);  
	}  
	=====================================================================  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			size(t.members)  
		From  
			Team t  */ select  
				(select  
					count(members1_.TEAM_ID)  
				from  
					Member members1_  
				where  
					team0_.id = members1_.TEAM_ID) as col_0_0_  
			from  
				Team team0_  
	=====================================================================  
  
## 사용자 정의 함수  
	- 하이버네이트는 사용전 방언에 추가해야 함  
	- 사용하는 DB 방언을 상속 받고, 사용자 정의 함수를 등록함  
	ex) select function('group_concat' , i.name) from Item i  
  
	- H2Dialect 클래스를 상속받아서 구현  
	=====================================================================  
	package dialect;  
  
	import org.hibernate.dialect.H2Dialect;  
	import org.hibernate.dialect.function.StandardSQLFunction;  
	import org.hibernate.type.StandardBasicTypes;  
  
	public class MyH2dialect extends H2Dialect {  
		public MyH2dialect(){  
			//DB에 있는 Function 등록  
			registerFunction("group_concat",new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));  
		}  
	}  
	=====================================================================  
  
	- 구현한 클래스를 persistence.xml 에 등록  
	=====================================================================  
  
	<!-- 기존 -->  
	<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>  
  
	<!-- 변경 -->  
	<property name="hibernate.dialect" value="dialect.MyH2dialect"/>  
	=====================================================================  
  
	상속받는 H2Dialect 클래스를 참고하여 등록 하는 방법을 확인  
	=====================================================================  
	package org.hibernate.dialect;  
  
	public class H2Dialect extends Dialect {  
		...중략...  
		this.registerFunction("right", new StandardSQLFunction("right", StandardBasicTypes.STRING));  
        this.registerFunction("rtrim", new StandardSQLFunction("rtrim", StandardBasicTypes.STRING));  
        this.registerFunction("soundex", new StandardSQLFunction("soundex", StandardBasicTypes.STRING));  
        this.registerFunction("space", new StandardSQLFunction("space", StandardBasicTypes.STRING));  
  
	=====================================================================  
  
	- 실행 코드  
	=====================================================================  
	String query = "select function('group_concat',m.username) From Member m";  
  
	List<String> result = em.createQuery(query,String.class)  
			.getResultList();  
	for(String a : result){  
		System.out.println(a);  
	}  
	=====================================================================  
	※ String query = "select group_concat(m.username) From Member m"; 쿼리로도 됨  
  
	- 실행 쿼리  
	=====================================================================  
	Hibernate:  
		/* select  
			function('group_concat',  
			m.username)  
		From  
			Member m */ select  
				group_concat(member0_.username) as col_0_0_  
			from  
				Member member0_  
	김성철,양혜은  
  
	=====================================================================  

{% endraw %}
