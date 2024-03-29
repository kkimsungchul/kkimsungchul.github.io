---
layout: post
title: "[Spring Boot] 설정 우선 순위(profile)"
subtitle: "Spring Boot 설정 우선 순위(profile)"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## SpringBoot - 설정 우선 순위(profile)  
  
	1. Command-line arguments (Command-line 인자):  
		스프링 애플리케이션을 실행할 때, Command-line 인자로 설정을 전달할 수 있습니다.  
		이러한 인자들은 애플리케이션의 설정에 우선하여 적용됩니다.  
		예를 들어, --spring.profiles.active=production과 같은 방식으로 프로파일을 지정할 수 있습니다.  
  
	2. Java System Properties (Java 시스템 속성):  
		JVM을 실행할 때 -D 옵션을 사용하여 Java 시스템 속성으로 설정을 전달할 수 있습니다.  
		스프링은 이러한 시스템 속성을 로드하여 애플리케이션 설정에 우선하여 적용합니다.  
		-DSpring.profiles.active=dev  
  
	3. OS Environment variables (환경 변수):  
		운영체제의 환경 변수를 통해 설정을 전달할 수 있습니다.  
		스프링은 이러한 환경 변수를 읽어와 설정에 우선하여 적용합니다.  
  
	4. External Configuration Files (외부 설정 파일):  
		스프링은 프로퍼티 파일인 application.properties 또는 application.yml 파일을 사용하여 설정을 관리합니다.  
		이러한 설정 파일은 애플리케이션의 classpath 루트 또는 config 디렉토리에 위치할 수 있습니다.  
		외부 설정 파일은 프로젝트 내부의 설정보다 우선하여 적용됩니다.  
  
	5. Profile-specific Properties (프로파일별 설정):  
		스프링은 프로파일별로 설정을 구성할 수 있습니다.  
		예를 들어, application-dev.properties 파일은 dev 프로파일에 대한 설정을 담고 있습니다.  
		프로파일은 spring.profiles.active 속성을 사용하여 활성화하며, 이 설정들은 일반적인 외부 설정보다 우선합니다.  
  
	6. Default Properties (기본 설정):  
		스프링은 기본적으로 application.properties 파일을 찾고, 해당 파일에 정의된 기본 설정을 적용합니다.  
		이 설정들은 다른 외부 설정들보다 우선됩니다.  
  
	7. @PropertySource 어노테이션:  
		@PropertySource 어노테이션을 사용하여 추가적인 프로퍼티 파일을 로드할 수 있습니다.  
		이 설정들은 기본 설정보다 우선합니다.  

{% endraw %}
