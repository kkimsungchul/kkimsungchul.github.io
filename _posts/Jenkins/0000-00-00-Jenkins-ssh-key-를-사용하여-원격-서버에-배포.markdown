---
layout: post
title: "[Jenkins] ssh key 를 사용하여 원격 서버에 배포"
subtitle: "Jenkins ssh key 를 사용하여 원격 서버에 배포"
date: 2023-01-01 00:00:00 +0900
categories: Jenkins
---
{% raw %}
## Jenkins - ssh key를 사용하여 원격 서버에 배포  
  
	참고링크 :  
		https://ktaes.tistory.com/100  
  
	1. Jenkins 서버 - Jenkins 계정으로 로그인 후 RSA키 생성 명령어 입력  
		ssh-keygen -t rsa  
  
	2. Jenkins 관리 - Plugin Manager - 설치 가능에서 SSH plugin 을 설치한다.  
  
	3. Jenkins 관리 - Manage Credential 에서 Add Credential 클릭  
  
		Kind : SSH Username with private key  
		ID : 중복되지 않는 인증 ID - 해당 ID 값으로 pipline에서 인증 정보를 사용  
		username : 생략  
		private key : ssh-keygen 으로 생성한 private key 내용 (ex: jenkins_rsa)  
					  cat jenkins_rsa 명령어로 출력되는 내용  
		passphrase : ssh-keygen으로 인증키 생성시 입력한 password  
  
	4. Jenkins 관리 - 시스템 설정 - SSH remote hosts 단락으로 이동 후 추가버튼 클릭하여 아래의 화면의 내용 채우기  
		Hostname : 접속할 서버의 IP  
		Port : 접속할 서버의 포트 (기본 22)  
		Credentials : 3번 항목에서 생성한 Credential 선택  
  
	5. 배포할 서버 - tomcat 계정의 Home 디렉토리 확인  
		cat /etc/passwd | grep tomcat  
  
	6. 배포할 서버 - tomcat 계정의 home 디렉토리에 authroized_keys 파일 생성  
  
		cd /home/tomcat/.ssh			//경로 이동 - 없을경우 mkdir 로 생성 후 소유자와 그룹을 사용하려는 계정으로 변경  
		vi authroized_keys				//파일 생성  
  
	7. 배포할 서버 - authroized_keys 파일에 Jenkins 서버에서 생성한 id_ras.pub 파일의 내용을 붙여넣기 후 저장  
  
	8. 배포할 서버 - authroized_keys 파일의 권한은 600 , 소유자는 로그인 할 계정 (tomcat)  
		chmod 600 authroized_keys  
  
	9. Jenkins 관리 - 시스템설정 - SSH remote Hosts 에서 설정한 내용으로가 Check connection 클릭하여 접속 확인  
  
	10. 접속 확인이 완료되면 Jenkins job 생성  
		-> New Item  
		-> Freestyle project  
  
	11. Jenkins job 설정  
		11-1. Build 부분에서 "Add build step" 선택 후 "Execute shell script on remote host using ssh" 클릭  
  
		11-2. SSH site 는 4번 항목에서 생성한 host 의 정보 선택  
			ex) 계정명@IP  
				tomcat@192.168.0.111  
  
		11-3. 바로 하단의 Command 부분에 실행할 스크립트 작성  
  

{% endraw %}
