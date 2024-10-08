---
layout: post
title: "[DB] postgresql 용량 확보"
subtitle: "DB postgresql 용량 확보"
date: 2023-01-01 00:00:00 +0900
categories: DB
---
{% raw %}
## DB - postgresql 용량 확보  
  
## 참고 링크  
	https://techblog.woowahan.com/9478/  
	https://nick2ya.tistory.com/4  
	https://blog.gaerae.com/2015/09/postgresql-vacuum-fsm.html  
	https://hochoon-dev.tistory.com/entry/Postgresql-Vacuum  
	https://postgresql.kr/docs/9.4/routine-vacuuming.html  
  
## 문제 발생  
	사용중인 디스크의 용량이 꽉차버림  
	df -h 명령어를 사용해서 용량을 확인해보니  
	20G중 잔여용량이 100M 였음  
  
	drop 테이블을 했지만 많이 증가하진않았음  
	100M -> 150M  
  
	용량이 많은 테이블들은 따로 있는데, 이 테이블은 drop 할수가 없음  
	그래서 해당 테이블에서 오래된 자료들을 싹 삭제함  
	삭제하기전에는 2.8GB를 사용하고 있었고, 약 20만개의 행을 delete함  
  
	delete 후 용량을 확인해보니 그대로임  
	이해가 되지않아 내용을 찾아보니 Postgresql에서는 update delete 시 해당 데이터를 지우거나 업데이트하는게 아닌 insert 방식처럼 작동한다고 함  
  
	그래서 배큠(vacuum)이라는 명령어를 찾았고, 진행을 하는데  
	공간이 부족하다는 오류메시지가 나옴..  
		SQL state: 53100  
		Detail: Could not write to file "pg_subtrans/4E3E" at offset 155648: No space left on device.  
  
	배큠 자체에도 디스크가 필요함..  
		cuum full 동작은 대상 테이블을 1벌 더 COPY하는 식으로 동작하기 때문에  
		디스크 가용량이 여유롭지 못한 상황에서는 시도조차 할 수 없습니다.  
	결국 데이터가 적은 테이블부터 하나씩 전부다 진행해서 용량 확보했음  
	150M -> 950M  
  
## 발생 원인  
	Postgresql은 다른 RDB와 달리, UPDATE 쿼리와 DELETE 쿼리가 순전히 기존 데이터를 변형하거나 삭제를 하는 방향으로 동작하지 않는다. 즉, UPDATE 쿼리는 기존에 있던 데이터는 그대로 둔 채 새로운 데이터를 추가하는 방식으로 동작하며, DELETE 쿼리 또한 기존의 데이터를 삭제하지 않고 삭제해야 한다는 표시만 남겨두는 방식으로 동작을 한다. 이는 Postgresql이 동시접근에 의한 이슈들을 방지하기 위해 채택된 MVCC 모델을 적용하고 있기 때문이다.  
  
	MVCC : 동시접근을 허용하는 데이터베이스에서 동시성을 제어하기 위해 사용하는 방법.  
	즉, MVCC 모델에서 데이터에 접근하는 사용자는 접근한 시점에서의 데이터베이스의 Snapshot을 읽는데,  
	이 snapshot 데이터에 대한 변경이 완료될 때(트랜잭션이 commit 될 때)까지 만들어진 변경사항은 다른 데이터베이스 사용자가 볼 수 없다.  
	이러한 개념에 의해 사용자가 데이터를 업데이트하면 이전의 데이터를 덮어 씌우는게 아니라 새로운 버전의 데이터를 생성한다.  
	대신 이전 버전의 데이터와 비교해서 변경된 내용을 기록한다.  
	이렇게해서 하나의 데이터에 대해 여러 버전의 데이터가 존재하게 되고, 사용자는 마지막 버전의 데이터를 읽게 된다.  
  
## 옵션  
	vacuum : vacuum 수행중에도 select dml 작업 가능  
	단순히 사용 가능한 공간을 free space map에 반환, 실제 디스크 공간 확보는 안됨  
  
	full : 디스크상 여유공간 확보됨, 시간 오래걸림, 수행되는 테이블에 exclusive lock 발생  
	운영중인 DB에 사용 금지  
  
	analyze : 통계 메타데이터를 업데이트하므로 쿼리 옵티마이저가 더 정확한 쿼리 계획을 생성할수 있어 vacuum 명령어시 같이 실행하는게 좋음  
  
## 사용법  
	- DB 전체 실행  
	vacuum full analyze;  
  
	- DB 전체 간단하게 실행  
	vacuum verbose analyze;  
  
	- 특정 테이블만 간단하게 실행  
	vacuum analyse 테이블명  
  
	- 특정 테이블만 풀 실행  
	vacuum full 테이블명  
  
## 용량 확보 방법  
1. linux에서 postgresql에서 데이터를 저장중이 디렉토리에 접근하여 폴더명들을 확인  
  
	ex) /data/15332/5452  
		위와같이 경로가 되어있으면  
		15332는 데이터베이스  
		5452는 해당 데이터베이스에 있는 테이블임  
  
2. 용량이 큰 폴더명들을 postgreslq에서 조회  
	SELECT datname FROM pg_database WHERE oid = 16384;  
  
3. 해당 데이터베이스를 접속 후 테이블 조회  
	SELECT relname, relnamespace::regnamespace, relkind  
	FROM pg_class WHERE relfilenode = 16417;  
  
4. 테이블 명이 조회되면 Vacuum  명령어로 정리  
	Vacuum full analyse 테이블명  
  
## 알아두면 좋은점  
	테이블의 모든 자료를 아에 지워버리고자 한다면, DELETE 명령보다는 TRUNCATE 명령을 사용하는 것이 낫다.  
	이 명령은 작업 뒤에, VACUUM 이나 VACUUM FULL 명령을 사용할 필요가 없다.  
	단, 이 명령을 사용하게 되면 MVCC 대상에서 제외되기 때문에, 다중 세션 다중 트랜잭션 환경에서는 위험하다.  
  
	TRUNCATE 명령어에는 where 옵션이 없어 테이블에 있는 모든 데이터를 다 지워버림  

{% endraw %}
