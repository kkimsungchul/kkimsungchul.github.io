---
layout: post
title: "[JAVA] Robot 사용하기"
subtitle: "JAVA Robot 사용하기"
date: 2023-01-01 00:00:00 +0900
categories: JAVA_Spring
---
{% raw %}
## JAVA - robot 사용하기  
  
	API  
		https://docs.oracle.com/javase/7/docs/api/java/awt/Robot.html  
	참고 사이트  
		https://velog.io/@nkstar00/Java%EB%A1%9C-Macro%EB%A5%BC-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EC%9E%90  
  
	키보드 버튼 값 매핑  
		https://stackoverflow.com/questions/15313469/java-keyboard-keycodes-list/31637206  
  
	https://github.com/kkimsungchul/study/tree/master/JAVA%26Spring/%5BJAVA%5D%20Robot%20%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0  
  
	자바로도 매크로가 만들어 지나 테스트 해봄  
  
	keyPress 키보드를 누름  
	keyRelease 키보드를 땜  
	delay 지연시간 (단위 ms)  
	==================================================================================================================================================  
			Robot robot = new Robot();  
			int macroKey = "65";  
			robot.keyPress(macroKey);  
			robot.keyRelease(macroKey);  
			robot.delay(100);  
	==================================================================================================================================================  
  
	65는 키보드 A 임  
	대문자를 입력해야 할 경우, 대문자의 앞 뒤에 shift 를 넣어주면됨  
	, 로 구분하여서 특정문자들은 순서대로 입력하도록 설정하였음  
	==================================================================================================================================================  
  
	String macroWord = "windows,c,m,d,enter,i,p,shift,c,o,n,shift,f,i,g,enter";  
  
	==================================================================================================================================================  
  
## 마우스 클릭 이벤트  
  
	======================================================================================================  
        try{  
            Robot robot = new Robot();  
            robot.mouseMove(605, 85);  
            robot.mousePress(InputEvent.BUTTON1_DOWN_MASK);//좌클릭 다운  
            robot.mouseRelease(InputEvent.BUTTON1_DOWN_MASK);//좌클릭 업  
            robot.mousePress(InputEvent.BUTTON1_DOWN_MASK);//좌클릭 다운  
            robot.mouseRelease(InputEvent.BUTTON1_DOWN_MASK);//좌클릭 업  
  
        }catch(Exception e) {  
            System.out.println("실패");  
            e.printStackTrace();  
        }  
	======================================================================================================  

{% endraw %}
