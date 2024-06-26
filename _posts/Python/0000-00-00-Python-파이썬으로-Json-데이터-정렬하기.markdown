---
layout: post
title: "[Python] 파이썬으로 Json 데이터 정렬하기"
subtitle: "Python 파이썬으로 Json 데이터 정렬하기"
date: 2023-01-01 00:00:00 +0900
categories: Python
---
{% raw %}
## Python - 파이썬으로 json 데이터 정렬하기  
  
## 사용 이유  
	JSON 데이터를 웹에서 파싱할 경우 가끔 문자열 타입으로 한줄로 올때가 있음  
	아래와 같은 데이터를 보기 좋게 정렬하려고 함  
	ex)  
		{"users": [{"userId": 1,"firstName": "AAAAA","lastName": "as23","phoneNumber": "123456","emailAddress": "AAAAA@test.com","homepage": "https://amogg.tistory.com/1"},{"userId": 2,"firstName": "BBBB","lastName": "h5jdd","phoneNumber": "123456","homepage": "https://amogg.tistory.com/2"},{"userId": 3,"firstName": "CCCCC","lastName": "2dhbs","phoneNumber": "33333333","homepage": "https://amogg.tistory.com/3"},{"userId": 4,"firstName": "DDDDD","lastName": "bacasd","phoneNumber": "222222222","homepage": "https://amogg.tistory.com/4"},{"userId": 5,"firstName": "EEEEE","lastName": "asdfasdf","phoneNumber": "111111111","homepage": "https://amogg.tistory.com/5"}]}  
  
##  코드  
	==========================================================================  
	import json  
  
	## 원본 JSON 데이터  
	original_json = '''  
	{"users": [{"userId": 1,"firstName": "AAAAA","lastName": "as23","phoneNumber": "123456","emailAddress": "AAAAA@test.com","homepage": "https://amogg.tistory.com/1"},{"userId": 2,"firstName": "BBBB","lastName": "h5jdd","phoneNumber": "123456","homepage": "https://amogg.tistory.com/2"},{"userId": 3,"firstName": "CCCCC","lastName": "2dhbs","phoneNumber": "33333333","homepage": "https://amogg.tistory.com/3"},{"userId": 4,"firstName": "DDDDD","lastName": "bacasd","phoneNumber": "222222222","homepage": "https://amogg.tistory.com/4"},{"userId": 5,"firstName": "EEEEE","lastName": "asdfasdf","phoneNumber": "111111111","homepage": "https://amogg.tistory.com/5"}]}  
	'''  
	## 원본 JSON 파싱  
	data = json.loads(original_json)  
  
	## 포맷된 데이터를 들여쓰기와 함께 JSON으로 직렬화  
	formatted_json = json.dumps(data, indent=2, ensure_ascii=False)  
  
	## 포맷된 JSON을 텍스트 파일로 저장  
	with open('formatted_users1.json', 'w', encoding='utf-8') as file:  
		file.write(formatted_json)  
  
	print('저장완료')  
	==========================================================================  
  
## 결과  
	==========================================================================  
	{  
	  "users": [  
		{  
		  "userId": 1,  
		  "firstName": "AAAAA",  
		  "lastName": "as23",  
		  "phoneNumber": "123456",  
		  "emailAddress": "AAAAA@test.com",  
		  "homepage": "https://amogg.tistory.com/1"  
		},  
		{  
		  "userId": 2,  
		  "firstName": "BBBB",  
		  "lastName": "h5jdd",  
		  "phoneNumber": "123456",  
		  "homepage": "https://amogg.tistory.com/2"  
		},  
		{  
		  "userId": 3,  
		  "firstName": "CCCCC",  
		  "lastName": "2dhbs",  
		  "phoneNumber": "33333333",  
		  "homepage": "https://amogg.tistory.com/3"  
		},  
		{  
		  "userId": 4,  
		  "firstName": "DDDDD",  
		  "lastName": "bacasd",  
		  "phoneNumber": "222222222",  
		  "homepage": "https://amogg.tistory.com/4"  
		},  
		{  
		  "userId": 5,  
		  "firstName": "EEEEE",  
		  "lastName": "asdfasdf",  
		  "phoneNumber": "111111111",  
		  "homepage": "https://amogg.tistory.com/5"  
		}  
	  ]  
	}  
  
	==========================================================================  

{% endraw %}
