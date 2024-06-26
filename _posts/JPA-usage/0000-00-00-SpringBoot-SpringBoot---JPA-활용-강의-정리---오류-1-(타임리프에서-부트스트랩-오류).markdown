---
layout: post
title: "[SpringBoot] SpringBoot - JPA 활용 강의 정리 - 오류-1 (타임리프에서 부트스트랩 오류)"
subtitle: "SpringBoot SpringBoot - JPA 활용 강의 정리 - 오류-1 (타임리프에서 부트스트랩 오류)"
date: 2023-01-01 00:00:00 +0900
categories: JPA-usage
---
{% raw %}
## SpringBoot - JPA 활용 강의 정리 - 오류-1 (타임리프에서 부트스트랩 오류)  
  
https://www.inflearn.com/questions/27201/rebuild-%ED%95%B4%EB%8F%84-bootstrap-%EC%A0%81%EC%9A%A9-%EC%95%88-%EB%90%98%EC%8B%9C%EB%8A%94-%EB%B6%84%EB%93%A4%EA%BB%98  
  
## 에러 메시지  
	Failed to find a valid digest in the 'integrity' attribute for resource 'http://localhost:9090/css/bootstrap.min.css' with computed SHA-384 integrity 'T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN'. The resource has been blocked.  
  
## 조치 방안  
	참고 URL : https://www.inflearn.com/questions/27201/rebuild-%ED%95%B4%EB%8F%84-bootstrap-%EC%A0%81%EC%9A%A9-%EC%95%88-%EB%90%98%EC%8B%9C%EB%8A%94-%EB%B6%84%EB%93%A4%EA%BB%98  
	==========================================================================  
	크롬 브라우저 기준으로  
  
	다음과 같은 에러를 만나셨다면 아래 내용을 참고하시면 좋을 것 같습니다:)  
  
	"Failed to find a valid digest in the 'integrity' attribute for resource 'http://localhost:8080/css/bootstrap.min.css' with computed SHA-256 integrity 'L/W5Wfqfa0sdBNIKN9cG6QA5F2qx4qICmU2VgLruv9Y='. The resource has been blocked."  
  
	PDF에서 제공되는 소스를 복붙하시고 bootstrap 버전을 강사님과 동일한 버전을 쓰지 않았을 때 부트스트랩이 적용되지 않을 수 있습니다.  
  
	== fragments/header.html 중==  
  
	<link rel="stylesheet" href="/css/bootstrap.min.css"  integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">  
  
	해당 태그의 속성 중 "integrity"의 값이 현재 사용중인 부트스트랩 버전의 것과 일치하지 않으면 브라우저에서 block됩니다.  
  
	만약, 최신버전의 부트스트랩을 사용중이시라면  
  
	부트스트랩 다운로드 페이지 아래쪽에 "Bootstrap CDN" 항목이 있습니다. 그곳에서 제공하는 소스 중 위에서 언급한 "integrity" 속성의 값을 복사해서 프로젝트의 것과 교체해주시면 됩니다.  
  
	* css파일이랑 js파일 둘의 integrity 값이 다르니깐 잘 보시고 복사하세요.  
	==========================================================================  
  
## URL  
	https://getbootstrap.com/docs/5.3/getting-started/download/  
	위의 URL에서 integrity 속성값 복사  

{% endraw %}
