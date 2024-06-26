---
layout: post
title: "[Spring Boot] 파일 다운로드(file download)"
subtitle: "Spring Boot 파일 다운로드(file download)"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## SpringBoot - 파일 다운로드(File download)  
  
## 사용 이유  
	SpringBoot로 생성한 프로젝트를 서버에서 돌릴때 로그를 직접 서버에 들어가서 봐야되는 불편함이 있음.  
	왜 불편하냐면.. 들어가려면 내부망 PC로 서버 접근제어 신청하고,, 서버에 들어가서 봐야함  
	그래서 그냥 통째로 로그파일 내려받는 기능을 간단하게 구현함  
  
## 구현 방법  
	별도의 서비스를 만들지 않고,, 그냥 컨트롤러에 다 넣어놓았음  
	import는 기존에 자동적으로 생성되는 애들외에 파일 다운로드에 필요한 라이브러리는 주석으로 옆에 표시  
  
	=====================================================================  
	import org.springframework.core.io.FileSystemResource;		//다운로드 기능을 위해 추가  
	import org.springframework.core.io.Resource;				//다운로드 기능을 위해 추가  
	import org.springframework.http.HttpHeaders;				//다운로드 기능을 위해 추가  
	import org.springframework.http.MediaType;					//다운로드 기능을 위해 추가  
	import org.springframework.http.ResponseEntity;  
	import org.springframework.stereotype.Controller;  
	import org.springframework.web.bind.annotation.GetMapping;  
	import org.springframework.web.bind.annotation.PathVariable;  
  
	@RestController  
	public class SystemUtil{  
  
		@GetMapping("/logFileDown")  
		public ResponseEntity<Resource> fileDown(){  
			String filePath = "/tomcat/log/catalina.out";  
			Resource fileResource = new FileSystemResource(filePath);  
			HttpHeaders headers = new HttpHeaders();  
			headers.setContentDispositionFormData("attachment","tomcat.log"); //다운로드 시 파일명 지정  
			headers.setContentType(MediaType.parseMediaType("text/plain"));  //파일 형식 지정  
  
			return ResponseEntity  
				.ok()  
				.headers(headers)  
				.body(fileResource);  
		}  
	}  
  
	=====================================================================  
  

{% endraw %}
