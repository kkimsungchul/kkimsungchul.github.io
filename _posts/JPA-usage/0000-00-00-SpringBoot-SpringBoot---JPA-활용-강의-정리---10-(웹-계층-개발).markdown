---
layout: post
title: "[SpringBoot] SpringBoot - JPA 활용 강의 정리 - 10 (웹 계층 개발)"
subtitle: "SpringBoot SpringBoot - JPA 활용 강의 정리 - 10 (웹 계층 개발)"
date: 2023-01-01 00:00:00 +0900
categories: JPA-usage
---
{% raw %}
## SpringBoot - JPA 활용 강의 정리 - 10 (주문 검색 기능 개발)  
  
## 계층형 구조 사용  
	- controller , web : 웹 계층  
	- service : 비즈니스 로직 , 트랜잭션 처리  
	- repository : JPA를 직접 사용하는 계층 ,  엔티티 매니저 사용  
	- domain : 엔티티가 모여 있는 계층 , 모든 계층에서 사용  
  
## 패키지 구조  
	- jpabook.jpashop  
		- jomain  
		- exception  
		- repository  
		- service  
		- web  
  
## 개발순서  
	서비스 , 리포지토리 계층을 개발하고, 테스트 케이스를 작성해서 검증, 마지막에 웹 계층 적용  
  

{% endraw %}
