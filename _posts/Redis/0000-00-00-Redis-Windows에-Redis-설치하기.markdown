---
layout: post
title: "[Redis] Windows에 Redis 설치하기"
subtitle: "Redis Windows에 Redis 설치하기"
date: 2023-01-01 00:00:00 +0900
categories: Redis
---
{% raw %}
## Redis - Windows에 Redis 설치하기  
  
## 참고 링크  
	https://ittrue.tistory.com/318  
	https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-Window10-%ED%99%98%EA%B2%BD%EC%97%90-Redis-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0  
	https://firstws.tistory.com/52  
  
## Redis 다운로드  
	https://github.com/microsoftarchive/redis/releases  
	https://github.com/microsoftarchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.msi  
  
## 참고사항  
	아래 문서는 '윈도우 서비스' 와 '수동 실행' 두개로 나눠서 작성하였으며  
	conf파일 수정 부분은 똑같기에 윈도우 서비스에서 사용하는 부분에만 작성하였음  
	그 외 두개의 실행 방식에서의 차이점은 각각 작성함  
  
[ 윈도우 서비스로 Redis 사용 ]  
  
## Redis-x64-3.0.504.msi 파일 실행  
	실행 후 Next를 누르다보면 설정이 나옴  
	아래와 같이 변경  
	=====================================================================  
	Port : 6379 (기본)  
	Max Memory : 300 (기본 100)  
	=====================================================================  
  
## Redis 실행 확인 (윈도우 서비스 이용)  
	1. 실행 창(윈도우키 + R )에서 services.msc 입력 후 엔터  
	2. Redis 로 실행중인 서비스 확인  
  
## Redis cli 사용 하여 접속  
	1. 설치한 경로 이동  
		C:\Program Files\Redis  
	2. redis-cli.exe 실행  
  
	3. ping 명령어 입력  
		=====================================================================  
		127.0.0.1:6379> ping  
		PONG  
		=====================================================================  
  
## Redis port (윈도우 서비스 이용)변경  
	redis.windows-service.conf 파일을 열어 아래와 같이 수정  
	※ 설정 변경 후 윈도우 서비스 재시작  
	=====================================================================  
	## If port 0 is specified Redis will not listen on a TCP socket.  
	port 16379  
	=====================================================================  
  
## Redis 외부접속 허용  
	redis.windows-service.conf 파일을 열어 아래와 같이 수정  
	※ 설정 변경 후 윈도우 서비스 재시작  
	=====================================================================  
	## bind 192.168.1.100 10.0.0.1  
	## bind 127.0.0.1  
	bind 0.0.0.0  
	=====================================================================  
  
## Redis 비밀번호 설정  
	redis.windows-service.conf 파일을 열어 아래와 같이 수정  
	※ 설정 변경 후 윈도우 서비스 재시작  
	=====================================================================  
	## requirepass foobared  
	requirepass 원하는_비밀번호_입력  
	=====================================================================  
  
## Redis 접속  
	※ 포트번호를 변경하고 나면 redis-cli.exe 파일로 접속 불가  
	1. redis-cli.exe 가 있는 폴더로 이동  
		cd C:\Program Files\Redis  
	2. 아래의 명령어 입력  
	redis-cli -h {호스트명} -p {포트번호}  
		=====================================================================  
		redis-cli -h localhost -p 16379  
		=====================================================================  
	3. 비밀번호를 설정하였을 경우 오류 확인  
		비밀번호를 설정하였을 경우 접속 후 명령어를 입력하면 아래처럼 나옴  
		=====================================================================  
		localhost:16379> ping  
		(error) NOAUTH Authentication required.  
		=====================================================================  
  
	4. 아래의 순서로 다시 접속  
		redis-cli -h localhost -p 16379  
		auth 비밀번호_입력  
		=====================================================================  
		C:\Program Files\Redis>redis-cli -h localhost -p 16379  
		localhost:16379> auth 비밀번호_입력  
		OK  
		localhost:16379> ping  
		PONG  
		=====================================================================  
  
######################################################################  
  
[ Windows에서 수동으로 Redis 실행 ]  
  
## Redis 실행 (수동 실행)  
	1. 설치한 경로 이동  
		C:\Program Files\Redis  
	2. redis-server.exe 파일 실행  
  
## Redis port (수동 실행)변경  
	1. redis.windows.conf 파일열 열어서 아래와 같이 수정  
	=====================================================================  
	port 16379  
	=====================================================================  
	2. redis-server.exe 실행  
	실행 시 아래와 같이 경고가 뜨면서 포트번호가 변경이 안됌.  
	=====================================================================  
	[99276] 21 Sep 04:25:31.744 ## Warning: no config file specified, using the default config.  
	In order to specify a config file use C:\Program Files\Redis\redis-server.exe /path/to/redis.conf  
	=====================================================================  
  
	3. 명령어로 redis-server.exe 실행  
		3-1. redis-server.exe 가 있는 폴더로 이동  
			cd C:\Program Files\Redis  
		3-2. 아래의 명령어로 Redis 서버 실행  
			redis-server redis.windows.conf  
## Redis 접속  
	※ 포트번호를 변경하고 나면 redis-cli.exe 파일로 접속 불가  
	1. redis-cli.exe 가 있는 폴더로 이동  
		cd C:\Program Files\Redis  
	2. 아래의 명령어 입력  
	redis-cli -h {호스트명} -p {포트번호}  
	=====================================================================  
	redis-cli -h localhost -p 16379  
	=====================================================================  
  

{% endraw %}
