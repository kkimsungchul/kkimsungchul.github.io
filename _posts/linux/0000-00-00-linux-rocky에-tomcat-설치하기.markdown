---
layout: post
title: "[linux] rocky에 tomcat 설치하기"
subtitle: "linux rocky에 tomcat 설치하기"
date: 2023-01-01 00:00:00 +0900
categories: linux
---
{% raw %}
## rocky linux에 톰캣 설치  
  
## 톰캣 설치  
	- GPT랑 구글링으로 설치했음  
	- 설치한 버전은 10.1.28  
	https://downloads.apache.org/tomcat/tomcat-10/v10.1.28/  
## 계정 생성  
	※ 아래의 명령어는 root로 실행  
	useradd -r -m -U -d /opt/tomcat -s /bin/bash tomcat  
	-r: 시스템 계정으로 생성합니다.  
	-m: 홈 디렉토리를 생성합니다.  
	-U: 계정과 동일한 이름의 그룹을 생성합니다.  
	-d /opt/tomcat: /opt/tomcat을 홈 디렉토리로 설정합니다.  
	-s /bin/bash: 이 계정으로 로그인할 수 없도록 설정합니다.  
  
## 계정 비밀번호 생성  
	※ root 로 아래의 명령어 입력  
	=====================================================================  
	passwd tomcat  
	새 암호 :  
	새 암호 재입력 :  
	=====================================================================  
	위와 같이 설정하면 이제 일반 계정에서도 tomcat 계정으로 로그인이 가능함  
  
## tomcat 다운로드 및 압축풀기  
	※ 아래의 명령어는 root로 실행  
	cd /tmp  
	wget https://downloads.apache.org/tomcat/tomcat-10/v10.1.28/bin/apache-tomcat-10.1.28.tar.gz  
	tar -xf apache-tomcat-10.1.28.tar.gz -C /opt/  
  
## 경로 이동  
	※ 하다보니 이렇게 됨, 처음부터 tomcat 밑에 풀었으면 됐음  
	mv /opt/apache-tomcat-10.1.28 /opt/tomcat  
  
## 권한 및 소유자 변경  
	chown -R tomcat: /opt/tomcat  
	chmod -R u+x /opt/tomcat/apache-tomcat-10.1.28/bin  
  
## tomcat 실행  
	※ 여기서부터는 tomcat 계정으로 진행  
	su - tomcat  
	cd /opt/tomcat/apache-tomcat-10.1.28/bin  
	./startup.sh  
  
## tomcat 실행 확인  
	ps -ef |grep tomcat  
	[tomcat@localhost bin]$ ps -ef |grep tomcat  
	root       34515   34478  0 03:18 pts/0    00:00:00 su - tomcat  
	tomcat     34516   34515  0 03:18 pts/0    00:00:00 -bash  
	tomcat     34556       1 99 03:19 pts/0    00:00:07 /opt/jdk-17.0.2/bin/java -Djava.util.logging.config.file=/opt/tomcat/apache-tomcat-10.1.28/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED -classpath /opt tomcat/apache-tomcat-10.1.28/bin/bootstrap.jar:/opt/tomcat/apache-tomcat-10.1.28/bin/tomcat-juli.jar -Dcatalina.base=/opt/tomcat/apache-tomcat-10.1.28 -Dcatalina.home=/opt/tomcat/apache-tomcat-10.1.28 -Djava.io.tmpdir=/opt/tomcat/apache-tomcat-10.1.28/temp org.apache.catalina.startup.Bootstrap start  
	tomcat     34596   34516  0 03:19 pts/0    00:00:00 ps -ef  
	tomcat     34597   34516  0 03:19 pts/0    00:00:00 grep --color=auto tomcat  
  
## tomcat 실행 확인 2  
	[tomcat@localhost bin]$ curl localhost:8080  
	<!doctype html><html lang="ko"><head><title>HTTP 상태 404 – 찾을 수 없음</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:##525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP 상태 404 – 찾을 수 없음</h1><hr class="line" /><p><b>타입</b> 상태 보고</p><p><b>메시지</b> 파일 [&#47;index.jsp]을(를) 찾을 수 없습니다.</p><p><b>설명</b> Origin 서버가 대상 리소스를 위한 현재의 representation을 찾지 못했거나, 그것이 존재하는지를 밝히려 하지 않습니다.</p><hr class="line" /><h3>Apache Tomcat/10.1.28</h3></body></html>[tomcat@localhost bin]$  
  
## tomcat 로그 파일 확인  
	cd /opt/tomcat/apache-tomcat-10.1.28/logs/  
	vi catalina.out  
  
	또는  
	tail -f /opt/tomcat/apache-tomcat-10.1.28/logs/catalina.out  
  
## 방화벽 오픈  
	※ root로 진행  
	firewall-cmd --zone=public --permanent --add-port=8080/tcp  
	firewall-cmd --reload  
  
## 외부 접속 확인  
	http://linuxip:8080  
  
## 포트번호 변경  
	- 8080 쓰려고 했는데 공유기에서 지가쓴다고 설정못하게 함  
	- 18080으로 변경하고 방화벽 오픈 작업 진행  
  
	cd /opt/tomcat/apache-tomcat-10.1.28/conf  
	vi server.xml  
	=====================================================================  
    <Connector port="8080" protocol="HTTP/1.1"  
               connectionTimeout="20000"  
               redirectPort="8443"  
               maxParameterCount="1000"  
               />  
	=====================================================================  
  
	아래와 같이 변경  
	=====================================================================  
    <Connector port="18080" protocol="HTTP/1.1"  
               connectionTimeout="20000"  
               redirectPort="18443"  
               maxParameterCount="1000"  
               />  
	=====================================================================  
  
	- 방화벽 오픈  
	※ root 계정으로 진행  
	firewall-cmd --zone=public --permanent --add-port=18080/tcp  
	firewall-cmd --reload  
  
## 언어 변경  
	- 이거 tomcat 다운로드 하고 그냥 실행하면 한글로 로그가 찍힘  
	- 한글이라서 편하긴 한데 오류나서 검색하거나 디버깅이 필요할경우에는 난감한 상황이 생길 수 있음  
	export JAVA_OPTS="$JAVA_OPTS -Duser.language=en" 내용을 추가해주면 됨  
  
	cd /opt/tomcat/apache-tomcat-10.1.28/bin  
	vi startup.sh  
	=====================================================================  
	esac  
	## esac 바로 아래의 내용을 추가  
	export JAVA_OPTS="$JAVA_OPTS -Duser.language=en"  
	=====================================================================  
  
	- 기존  
	=====================================================================  
	10-Aug-2024 03:19:28.280 SEVERE [main] org.apache.jasper.EmbeddedServletOptions.<init> 귀하가 지정한 scratchDir [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/host-manager]은(는) 사용할 수 없습니다.  
	10-Aug-2024 03:19:28.282 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/host-manager]에 대한 배치가 [77] 밀리초에 완료되었습니다.  
	10-Aug-2024 03:19:28.283 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager]을(를) 배치합니다.  
	10-Aug-2024 03:19:28.290 WARNING [main] org.apache.catalina.core.StandardContext.postWorkDirectory 컨텍스트 [/manager]을(를) 위한 작업 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/manager]을(를) 생성하지 못했습니다.  
	10-Aug-2024 03:19:28.349 INFO [main] org.apache.jasper.servlet.TldScanner.scanJars 적어도 하나의 JAR가 TLD들을 찾기 위해 스캔되었으나 아무 것도 찾지 못했습니다. 스캔했으나 TLD가 없는 JAR들의 전체 목록을 보시려면, 로그 레벨을 디버그 레벨로 설정하십시오. 스캔 과정에서 불필요한 JAR들을 건너뛰면, 시스템 시작 시간과 JSP 컴파일 시간을 단축시킬 수 있습니다.  
	10-Aug-2024 03:19:28.353 SEVERE [main] org.apache.jasper.EmbeddedServletOptions.<init> 귀하가 지정한 scratchDir [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/manager]은(는) 사용할 수 없습니다.  
	10-Aug-2024 03:19:28.355 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager]에 대한 배치가 [71] 밀리초에 완료되었습니다.  
	10-Aug-2024 03:19:28.367 INFO [main] org.apache.coyote.AbstractProtocol.start 프로토콜 핸들러 ["http-nio-8080"]을(를) 시작합니다.  
	10-Aug-2024 03:19:28.392 INFO [main] org.apache.catalina.startup.Catalina.start 서버가 [1462] 밀리초 내에 시작되었습니다.  
	10-Aug-2024 03:19:53.817 SEVERE [http-nio-8080-exec-1] org.apache.jasper.JspCompilationContext.createOutputDir JSP 컴파일에 필요한 출력 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/ROOT/org/apache/jsp/](을)를 생성할 수 없습니다.  
  
	=====================================================================  
  
	- 변경 후  
	=====================================================================  
	[tomcat@localhost bin]$ tail -f /opt/tomcat/apache-tomcat-10.1.28/logs/catalina.out  
	10-Aug-2024 03:40:34.444 INFO [main] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.  
	10-Aug-2024 03:40:34.529 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/tomcat/apache-tomcat-10.1.28/webapps/examples] has finished in [419] ms  
	10-Aug-2024 03:40:34.530 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/opt/tomcat/apache-tomcat-10.1.28/webapps/host-manager]  
	10-Aug-2024 03:40:34.591 INFO [main] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.  
	10-Aug-2024 03:40:34.601 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/tomcat/apache-tomcat-10.1.28/webapps/host-manager] has finished in [70] ms  
	10-Aug-2024 03:40:34.602 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager]  
	10-Aug-2024 03:40:34.668 INFO [main] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.  
	10-Aug-2024 03:40:34.672 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager] has finished in [70] ms  
	10-Aug-2024 03:40:34.683 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]  
	10-Aug-2024 03:40:34.722 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [1409] milliseconds  
	=====================================================================  
  
## 혹시나 하는 내용  
	계정 변경을 하지 않고 tomcat을 root로 기동할 경우 다시 tomcat계정으로 실행하면 문제가 발생함  
	1. log 파일들의 소유자는 root로 되어버림  
		이럴경우 다시 tomcat으로 구동하면 permission denied 가 뜰수있음  
		이럴때는 log파일들의 소유자와 그룹을 다시 tomcat으로 변경해주면됨  
	2. 캐시파일 정리  
		아래의 경로에 캐시 파일이 생성됨  
		/opt/tomcat/apache-tomcat-10.1.28/work  
		소유자와 그룹을 확인해 보면 root로 되어 있음  
		=====================================================================  
		[root@localhost work]## ll  
		합계 0  
		drwxr-x---. 3 root root 23  8월 10 03:13 Catalina  
		=====================================================================  
		rm - rf /opt/tomcat/apache-tomcat-10.1.28/work/*  
		위 파일을 root로 삭제해버리고 나서 다시 tomcat 계정으로 서비스 실행  
  
## 톰캣 실행시 아래의 오류메시지 출력  
	위 내용은 톰캣을 tomcat계정이 아닌 다른 계정으로 실행했을 때 캐시가 남아서 안되는 경우이므로, /opt/tomcat/apache-tomcat-10.1.28/work 경로의 캐시파일 지우고 실행  
	=====================================================================  
	[root@localhost logs]## tail -f catalina.out  
	10-Aug-2024 03:19:28.280 SEVERE [main] org.apache.jasper.EmbeddedServletOptions.<init> 귀하가 지정한 scratchDir [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/host-manager]은(는) 사용할 수 없습니다.  
	10-Aug-2024 03:19:28.282 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/host-manager]에 대한 배치가 [77] 밀리초에 완료되었습니다.  
	10-Aug-2024 03:19:28.283 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager]을(를) 배치합니다.  
	10-Aug-2024 03:19:28.290 WARNING [main] org.apache.catalina.core.StandardContext.postWorkDirectory 컨텍스트 [/manager]을(를) 위한 작업 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/manager]을(를) 생성하지 못했습니다.  
	10-Aug-2024 03:19:28.349 INFO [main] org.apache.jasper.servlet.TldScanner.scanJars 적어도 하나의 JAR가 TLD들을 찾기 위해 스캔되었으나 아무 것도 찾지 못했습니다. 스캔했으나 TLD가 없는 JAR들의 전체 목록을 보시려면, 로그 레벨을 디버그 레벨로 설정하십시오. 스캔 과정에서 불필요한 JAR들을 건너뛰면, 시스템 시작 시간과 JSP 컴파일 시간을 단축시킬 수 있습니다.  
	10-Aug-2024 03:19:28.353 SEVERE [main] org.apache.jasper.EmbeddedServletOptions.<init> 귀하가 지정한 scratchDir [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/manager]은(는) 사용할 수 없습니다.  
	10-Aug-2024 03:19:28.355 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory 웹 애플리케이션 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/webapps/manager]에 대한 배치가 [71] 밀리초에 완료되었습니다.  
	10-Aug-2024 03:19:28.367 INFO [main] org.apache.coyote.AbstractProtocol.start 프로토콜 핸들러 ["http-nio-8080"]을(를) 시작합니다.  
	10-Aug-2024 03:19:28.392 INFO [main] org.apache.catalina.startup.Catalina.start 서버가 [1462] 밀리초 내에 시작되었습니다.  
	10-Aug-2024 03:19:53.817 SEVERE [http-nio-8080-exec-1] org.apache.jasper.JspCompilationContext.createOutputDir JSP 컴파일에 필요한 출력 디렉토리 [/opt/tomcat/apache-tomcat-10.1.28/work/Catalina/localhost/ROOT/org/apache/jsp/](을)를 생성할 수 없습니다.  
	=====================================================================  
  

{% endraw %}
