---
layout: post
title: "[Spring Boot] SpringBoot 3.x 버전 실행 오류"
subtitle: "Spring Boot SpringBoot 3.x 버전 실행 오류"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## Spring Boot - SpringBoot 3.x 버전 실행 오류  
  
## 오류 메시지  
  
	==========================================================================  
	No matching variant of org.springframework.boot:spring-boot-gradle-plugin:3.1.0 was found. The consumer was configured to find a runtime of a library compatible with Java 16, packaged as a jar, and its dependencies declared externally, as well as attribute 'org.gradle.plugin.api-version' with value '7.6' but:  
			  - Variant 'apiElements' capability org.springframework.boot:spring-boot-gradle-plugin:3.1.0 declares a library, packaged as a jar, and its dependencies declared externally:  
				  - Incompatible because this component declares an API of a component compatible with Java 17 and the consumer needed a runtime of a component compatible with Java 16  
				  - Other compatible attribute:  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
			  - Variant 'javadocElements' capability org.springframework.boot:spring-boot-gradle-plugin:3.1.0 declares a runtime of a component, and its dependencies declared externally:  
				  - Incompatible because this component declares documentation and the consumer needed a library  
				  - Other compatible attributes:  
					  - Doesn't say anything about its target Java version (required compatibility with Java 16)  
					  - Doesn't say anything about its elements (required them packaged as a jar)  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
			  - Variant 'mavenOptionalApiElements' capability org.springframework.boot:spring-boot-gradle-plugin-maven-optional:3.1.0 declares a library, packaged as a jar, and its dependencies declared externally:  
				  - Incompatible because this component declares an API of a component compatible with Java 17 and the consumer needed a runtime of a component compatible with Java 16  
				  - Other compatible attribute:  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
			  - Variant 'mavenOptionalRuntimeElements' capability org.springframework.boot:spring-boot-gradle-plugin-maven-optional:3.1.0 declares a runtime of a library, packaged as a jar, and its dependencies declared externally:  
				  - Incompatible because this component declares a component compatible with Java 17 and the consumer needed a component compatible with Java 16  
				  - Other compatible attribute:  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
			  - Variant 'runtimeElements' capability org.springframework.boot:spring-boot-gradle-plugin:3.1.0 declares a runtime of a library, packaged as a jar, and its dependencies declared externally:  
				  - Incompatible because this component declares a component compatible with Java 17 and the consumer needed a component compatible with Java 16  
				  - Other compatible attribute:  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
			  - Variant 'sourcesElements' capability org.springframework.boot:spring-boot-gradle-plugin:3.1.0 declares a runtime of a component, and its dependencies declared externally:  
				  - Incompatible because this component declares documentation and the consumer needed a library  
				  - Other compatible attributes:  
					  - Doesn't say anything about its target Java version (required compatibility with Java 16)  
					  - Doesn't say anything about its elements (required them packaged as a jar)  
					  - Doesn't say anything about org.gradle.plugin.api-version (required '7.6')  
  
	==========================================================================  
  
## 원인  
	해당 프로젝트는 데스크탑에서 개발하던 소스를 노트북에서 내려받았을때 발생하였음.  
	데스크탑은 JDK 17을 사용하고 있었으며,  
	노트북의 경우에는 다른 프로젝트가 JDK1.8을 사용하기에 기본JDK를 1.8로 설정하여 사용하고 있었음 (JDK17은 설치된 상태)  
	오류메시지를 보더라도 17버전이랑 호환된다고 나오고 있음.  
  
## 해결 방안  
	IDE에서 컴파일 할 JAVA의 버전을 17로 올려주면 해결이 됨.  
	JAVA 17버전은 이미 설치되어 있기에 아래의 내용으로 해결을 하였으며, 없을경우 설치 후 진행하면 됨  
  
	1. build.gradle 확인  
  
		build.gradle 파일에 sourceCompatibility = '17' 설정을 확인  
  
	2. IntelliJ의 Gradle JVM 버전 확인  
  
		Setting  
		-> Build , Execution, Deployment  
		-> Build Tools  
		-> Gradle 클릭  
  
		우측 하단의 Gradle의 Gradle JVM 버전을 JDK 17버전으로 변경 후 저장  
		※ 저장 후 재기동 해야함  
  
	3. Project Structure 의 java 버전 확인  
		프로젝트에 설정되어 있는 SDK의 버전이 17버전으로 적용되어 있는지 확인  
  
	4. 모든 설정이 끝나면 Gradle Refresh 를 실행하여 정상적으로 빌드 되는지 확인  

{% endraw %}
