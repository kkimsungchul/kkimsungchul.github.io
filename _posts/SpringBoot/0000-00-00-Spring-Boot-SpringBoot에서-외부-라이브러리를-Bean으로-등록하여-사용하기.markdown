---
layout: post
title: "[Spring Boot] SpringBoot에서 외부 라이브러리를 Bean으로 등록하여 사용하기"
subtitle: "Spring Boot SpringBoot에서 외부 라이브러리를 Bean으로 등록하여 사용하기"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## SpringBoot -  SpringBoot에서 외부 라이브러리를 Bean으로 등록하여 사용하기  
  
## 라이브러리를 Bean 으로 등록 하여 사용  
  
	외부 JAR 파일을 Spring Boot 프로젝트에 등록하여 사용하려면 아래의 단계를 따라 진행할 수 있습니다.  
  
	1. 외부 JAR 파일 생성:  
		외부에서 만든 JAR 파일을 빌드하여 생성합니다.  
  
	2. 외부 JAR 파일을 Spring Boot 프로젝트에 추가:  
		외부 JAR 파일을 Spring Boot 프로젝트 내의 libs 또는 lib 디렉토리와 같은 폴더에 복사합니다.  
  
	3. Spring Boot 프로젝트의 설정 클래스에 외부 JAR 파일의 Bean 등록:  
		Spring Boot의 @Configuration 어노테이션이 지정된 설정 클래스를 생성하고, 외부 JAR 파일에서 제공하는 Bean을 해당 클래스에 등록합니다.  
  
		java Copy code  
		=====================================================================  
		import org.springframework.context.annotation.Bean;  
		import org.springframework.context.annotation.Configuration;  
		import com.example.external.ExternalJarClass;  
  
		@Configuration  
		public class ExternalJarConfig {  
  
			@Bean  
			public ExternalJarClass externalJarClass() {  
				return new ExternalJarClass();  
			}  
		}  
		=====================================================================  
  
		위의 예시에서 com.example.external.ExternalJarClass는 외부 JAR 파일에서 가져와야 하는 클래스입니다.  
  
	4. 메인 클래스에서 외부 JAR 파일의 Bean 사용:  
		Spring Boot의 메인 애플리케이션 클래스에서 외부 JAR 파일의 Bean을 주입받아 사용할 수 있습니다.  
  
	java Copy code  
	=====================================================================  
	import org.springframework.boot.SpringApplication;  
	import org.springframework.boot.autoconfigure.SpringBootApplication;  
	import org.springframework.context.ApplicationContext;  
	import com.example.external.ExternalJarClass;  
  
	@SpringBootApplication  
	public class MyApplication {  
  
		public static void main(String[] args) {  
			ApplicationContext context = SpringApplication.run(MyApplication.class, args);  
			ExternalJarClass externalJarInstance = context.getBean(ExternalJarClass.class);  
  
			// 외부 JAR 파일의 Bean 사용  
			externalJarInstance.doSomething();  
		}  
	}  
	=====================================================================  
  
	5. 실행:  
		Spring Boot 애플리케이션을 실행하면 외부 JAR 파일의 Bean이 Spring 컨테이너에 등록되고, 메인 클래스에서 해당 Bean을 주입받아 사용할 수 있게 됩니다.  
		이렇게 하면 외부 JAR 파일을 Spring Boot 프로젝트에서 사용할 수 있습니다.  
		중요한 점은 외부 JAR 파일을 클래스 패스에 추가하고 Spring Bean으로 등록하여 사용하는 것입니다.  
		만약 클래스 로딩이나 의존성 충돌과 같은 문제가 발생하면, 해당 문제를 해결하는 추가 조치가 필요할 수 있습니다.  
  
## Autowired 로 사용  
  
	1. 의존성 주입을 위한 클래스 경로 설정: 외부 JAR 파일을 클래스 패스에 추가해야 Spring이 해당 클래스를 스캔하고 주입할 수 있습니다. 클래스 패스에 JAR 파일이 올바르게 위치하도록 확인하세요.  
  
	2. 컴포넌트 스캔 설정: @Autowired 어노테이션을 사용하여 외부 JAR 파일에서 정의한 Bean을 주입받으려면, 해당 JAR 파일의 패키지가 Spring의 컴포넌트 스캔 범위에 포함되어야 합니다. 이를 위해 @ComponentScan 어노테이션을 사용하여 스캔 범위를 확장할 수 있습니다.  
  
	3. Bean 등록 및 사용: 외부 JAR 파일에서 정의한 Bean을 Spring의 설정 클래스나 컴포넌트 클래스 내에서 @Autowired 어노테이션을 사용하여 주입받아 사용할 수 있습니다.  
  
		java Copy code  
		=====================================================================  
		import org.springframework.beans.factory.annotation.Autowired;  
		import org.springframework.stereotype.Component;  
		import com.example.external.ExternalJarClass;  
  
		@Component  
		public class MyComponent {  
  
			private final ExternalJarClass externalJarInstance;  
  
			@Autowired  
			public MyComponent(ExternalJarClass externalJarInstance) {  
				this.externalJarInstance = externalJarInstance;  
			}  
  
			public void doSomethingWithExternalJar() {  
				externalJarInstance.doSomething();  
			}  
		}  
		=====================================================================  
  
	위의 코드에서 MyComponent 클래스는 외부 JAR 파일에서 정의한 Bean을 @Autowired로 주입받아 사용하는 예시입니다.  
	ExternalJarClass는 외부 JAR 파일에 정의된 클래스 이름에 해당하는 클래스입니다.  
  
	주의해야 할 점은, @ComponentScan을 통해 외부 JAR 파일의 패키지를 스캔 범위에 포함시켜야 하며, 외부 JAR 파일이 스프링 컴포넌트 스캔의 대상이어야 합니다.  
	만약 이를 설정하지 않으면 @Autowired가 제대로 동작하지 않을 수 있습니다.  
  
	또한, 의존성 충돌 문제가 발생할 수 있으니 이에 대한 처리도 필요할 수 있습니다.  

{% endraw %}
