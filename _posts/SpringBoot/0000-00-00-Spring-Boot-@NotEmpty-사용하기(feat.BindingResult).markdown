---
layout: post
title: "[Spring Boot] @NotEmpty 사용하기(feat.BindingResult)"
subtitle: "Spring Boot @NotEmpty 사용하기(feat.BindingResult)"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## SpringBoot - @NotEmpty 사용하기(feat. BindingResult)  
	참고 링크 : https://sanghye.tistory.com/36  
  
	벨리데이션을 체크해야 할 경우 VO의 필드에 어노테이션으로 적용하여 바로 사용할수 있는 기능  
  
## 사용방법  
	javax.validation.constraints 패키지에 가보면 다양한 클래스들이 있으며  
	@NotNull , @Size , @NotEmty 등의 어노테이션을 사용할수 있음.  
  
	SpringBoot의 2.3버전 이상부터는 SpringBoot에 포함되어 있지 않아서 라이브러리를 추가해줘야 함  
	==========================================================================  
	implementation 'org.springframework.boot:spring-boot-starter-validation'  
	==========================================================================  
  
	DTO 나 VO 클래스의 필드에 어노테이션으로 선언가능하며, 해당 벨리데이션 체크에 걸릴 경우 에러메시지를 출력하도록 되어 있음  
  
	- VO에 적용한 모습  
	==========================================================================  
	import javax.validation.constraints.NotEmpty;  
  
	@Getter @Setter  
	public class MemberForm {  
  
		@NotEmpty(message = "회원 이름은 필수 입니다.")  
		private String name;  
		private String city;  
		private String street;  
		private String zipcode;  
  
	}  
	==========================================================================  
  
	- 해당VO를 넘겨받는 컨트롤러의 모습  
	==========================================================================  
    @PostMapping("/members/new")  
    public String create(@Valid MemberForm memberForm){  
  
	}  
	==========================================================================  
	컨트롤러에서는 @Valid 어노테이션을 추가해줘야 함  
  
	- 빈값으로 넘겼을때의 에러페이지  
	==========================================================================  
	Whitelabel Error Page  
	This application has no explicit mapping for /error, so you are seeing this as a fallback.  
  
	Mon Nov 27 14:51:10 KST 2023  
	There was an unexpected error (type=Bad Request, status=400).  
	Validation failed for object='memberForm'. Error count: 1  
	org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors  
	Field error in object 'memberForm' on field 'name': rejected value [];  
	codes [NotEmpty.memberForm.name,NotEmpty.name,NotEmpty.java.lang.String,NotEmpty];  
	arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [memberForm.name,name];  
	arguments []; default message [name]]; default message [회원 이름은 필수 입니다.]  
	==========================================================================  
  
	위의 에러를 보면 "회원 이름은 필수 입니다." 라고 작성한 에러 메시지가 화면에 그대로 넘어가는것을 볼수 있음  
  
## BindingResult 사용방법  
	위에서 javax의 validation 을 사용하여 오류메시지를 띄었지만, 저대로 띄우는것보다는 처리를 해주는것이 좋음  
	BindingResult를 사용하게되면 화면에서 받아온 데이터(MemberForm)와 Valid에서 걸린 에러가 같이 BindingResult에 추가되어 있음.  
	아래와 같이 사용할 경우 에러가 있을 경우 리다이렉트 시키며, MemberForm에서 받아온 데이터도 그대로 BindingResult 에 포함되어 있기에  
	화면단에서 입력한 값을 그대로 되돌려 줄수 있음  
	==========================================================================  
    @PostMapping("/members/new")  
    public String create(@Valid MemberForm memberForm, BindingResult result){  
        /*  
        * memberForm 에서 validation 을 걸어준 부분에 걸리게되면 오류가 발생함  
        * 그 오류를 BindingResult result에 담은뒤 코드가 실행됨  
        * result 에는 memberForm도 담겨서 넘어가게 됨  
        * */  
        if(result.hasErrors()){  
            return "/members/createMemberForm";  
        }  
  
	==========================================================================  

{% endraw %}
