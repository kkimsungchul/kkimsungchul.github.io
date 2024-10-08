---
layout: post
title: "[Docker] 도커에서 오픈텔레메트리(opentelemetry) 데모 실행하기"
subtitle: "Docker 도커에서 오픈텔레메트리(opentelemetry) 데모 실행하기"
date: 2023-01-01 00:00:00 +0900
categories: Docker
---
{% raw %}
## Docker - 도커에서 오픈텔레메트리(opentelemetry) 데모 실행하기  
	※약 30GB의 용량을 차지하므로 실행 시 주의  
  
## 공식 문서  
	https://opentelemetry.io/docs/demo/docker-deployment/  
  
## 오픈텔레메트리데모 클론  
	git clone https://github.com/open-telemetry/opentelemetry-demo.git  
  
## 도커 실행  
	윈도우에서 도커 데스크탑 실행  
  
## 도커 가상환경 저장 경로 설정  
	1. 우측 상단의 Settings 클릭 (톱니바퀴모양)  
	2. 좌측의 Resources 메뉴 진입  
	3. Disk image location 경로 설정  
  
## terminal 진입  
	도커 데스크탑 우측 하단의 terminal 클릭  
  
## 아래의 명령어 입력  
	클론받은 경로로 이동 후 아래의 명령어 입력  
	docker compose up --force-recreate --remove-orphans --detach  
	※ 이미지 빌드 및 실행에 시간 소요됨  
  
## 빌드 확인  
	도커 데스크탑의 좌측 메뉴에서 Images 클릭  
	-> 오텔 데모 이미지들이 보이면 정상적으로 실행된거임  
  
## 접속 테스트  
	Web store: http://localhost:8080/  
	Grafana: http://localhost:8080/grafana/  
	Load Generator UI: http://localhost:8080/loadgen/  
	Jaeger UI: http://localhost:8080/jaeger/ui/  
	Tracetest UI: http://localhost:11633/  
	※ only when using make start-odd  
  
## 외부에서도 같은 포트로 접속하기  
	데모의 docker-compose.yml 파일을 열어서 콜렉터 쪽설정을 보면 포트번호가 외부랑 매핑이 안되어 있음  
	"${OTEL_COLLECTOR_PORT_GRPC}" 이렇게 하나만 두면 내부에서는 해당 포트로 적용이 되고  
	외부에서는 랜덤하게 도커에서 할당하는 포트로 접속해야함  
  
	"${OTEL_COLLECTOR_PORT_GRPC}:${OTEL_COLLECTOR_PORT_GRPC}" 이렇게 외부와 내부 포트를 매핑시켜주면됨  
  
	- 기존  
	=====================================================================  
  otelcol:  
    image: ${COLLECTOR_CONTRIB_IMAGE}  
    container_name: otel-col  
    deploy:  
      resources:  
        limits:  
          memory: 200M  
    restart: unless-stopped  
    command: [ "--config=/etc/otelcol-config.yml", "--config=/etc/otelcol-config-extras.yml" ]  
    user: 0:0  
    volumes:  
      - ${HOST_FILESYSTEM}:/hostfs:ro  
      - ${DOCKER_SOCK}:/var/run/docker.sock:ro  
      - ${OTEL_COLLECTOR_CONFIG}:/etc/otelcol-config.yml  
      - ${OTEL_COLLECTOR_CONFIG_EXTRAS}:/etc/otelcol-config-extras.yml  
    ports:  
      - "${OTEL_COLLECTOR_PORT_GRPC}"  
      - "${OTEL_COLLECTOR_PORT_HTTP}"  
    depends_on:  
      - jaeger  
    logging: *logging  
    environment:  
      - ENVOY_PORT  
      - HOST_FILESYSTEM  
      - OTEL_COLLECTOR_HOST  
      - OTEL_COLLECTOR_PORT_GRPC  
      - OTEL_COLLECTOR_PORT_HTTP  
	=====================================================================  
  
	- 변경  
	=====================================================================  
  otelcol:  
    image: ${COLLECTOR_CONTRIB_IMAGE}  
    container_name: otel-col  
    deploy:  
      resources:  
        limits:  
          memory: 200M  
    restart: unless-stopped  
    command: [ "--config=/etc/otelcol-config.yml", "--config=/etc/otelcol-config-extras.yml" ]  
    user: 0:0  
    volumes:  
      - ${HOST_FILESYSTEM}:/hostfs:ro  
      - ${DOCKER_SOCK}:/var/run/docker.sock:ro  
      - ${OTEL_COLLECTOR_CONFIG}:/etc/otelcol-config.yml  
      - ${OTEL_COLLECTOR_CONFIG_EXTRAS}:/etc/otelcol-config-extras.yml  
    ports:  
      - "${OTEL_COLLECTOR_PORT_GRPC}:${OTEL_COLLECTOR_PORT_GRPC}"  
      - "${OTEL_COLLECTOR_PORT_HTTP}:${OTEL_COLLECTOR_PORT_HTTP}"  
    depends_on:  
      - jaeger  
    logging: *logging  
    environment:  
      - ENVOY_PORT  
      - HOST_FILESYSTEM  
      - OTEL_COLLECTOR_HOST  
      - OTEL_COLLECTOR_PORT_GRPC  
      - OTEL_COLLECTOR_PORT_HTTP  
	=====================================================================  
  
## 포트 변경 확인  
	 docker ps |grep otel  
	=====================================================================  
	 "/otelcol-contrib --…"   22 seconds ago   Up 21 seconds  
	 0.0.0.0:4317-4318->4317-4318/tcp, :::4317-4318->4317-4318/tcp  
	=====================================================================  

{% endraw %}
