---
layout: post
title: "[Spring Boot] JPA 강의 정리-30 (서브쿼리)"
subtitle: "Spring Boot JPA 강의 정리-30 (서브쿼리)"
date: 2023-01-01 00:00:00 +0900
categories: JPA
---
{% raw %}
## SpringBoot - JPA 강의 정리-30(서브쿼리)  
  
## 서브 쿼리 지원 함수  
	- [NOT] EXISTS (subquery) : 서브쿼리에 결과가 존재하면 참  
		- All | ANY | SOME (subquery)  
		- ALL 모두 만족하면 참  
		- ANY , SOME : 같은 의미로 조건을 하나라도 만족하면 참  
	- [NOT] IN (subquery) : 서브쿼리의 결과 중 하나라도 같은 것이 있으면 참  
  
## JPA 서브 쿼리 한계  
	- JPA 는 WHERE , HAVING 절에서만 서브 쿼리 사용 가능  
	- SELECT 절도 가능(하이버네이트에서 지원)  
	- FROM 절의 서브 쿼리는 현재 JPQL에서 불가능(JOIN 으로 풀어서 해결)  
		그래도 안되면  
		- native sql 로 작성  
		- 애플리케이션 단에서 해결  
		- 쿼리를 두번 실행하여 애플리케이션단에서 해결  

{% endraw %}
