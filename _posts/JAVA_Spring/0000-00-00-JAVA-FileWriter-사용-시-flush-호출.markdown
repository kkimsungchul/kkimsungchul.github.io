---
layout: post
title: "[JAVA] FileWriter 사용 시 flush 호출"
subtitle: "JAVA FileWriter 사용 시 flush 호출"
date: 2023-01-01 00:00:00 +0900
categories: JAVA_Spring
---
{% raw %}
## JAVA - FileWriter 사용 시 flush 호출  
	참고링크 : https://two.leojuly.com/6  
		https://docs.oracle.com/javase/8/docs/api/java/io/OutputStreamWriter.html##close--  
  
## FileWriter 사용방법  
  
	1. try - catch 사용  
	==========================================================================  
	File file = new File(파일경로);  
	FileWriter fileWriter = null;  
	try{  
		fileWriter = new FileWriter(file);  
		fileWriter.writer("데이터 내용");  
		fileWriter.close();  
	}catch(Exception e){  
  
	}  
  
	==========================================================================  
  
	2. try-with-resources 사용  
	==========================================================================  
	File file = new File(파일경로);  
  
	try (FileWriter fileWriter = new FileWriter(file)) {  
		fileWriter.write("데이터 내용");  
	} catch (Exception e) {  
		// 예외 처리  
	}  
	==========================================================================  
  
## flush()를 호출해야 하는가 - 서론  
	이 내용을 작성하게된 계기임.  
	"fileWriter 객체를 생성 후 fileWriter.flush()를 호출하지 않고 fileWriter.close()를 호출하면 오류가 발생할수 있다."  
	내가 작성하진 않았지만, 위에 FileWriter 사용방법 1에 작성한 try - catch 로 코드가 작성되어 있었음.  
	실제로 코드도 close()를 호출하는 순간에 파일에 내용이 적재되는 것을 확인하였음 (디버거 모드로 확인)  
  
	논리적으로 생각해봤을때 flush를 close() 할때 호출 하는 구나 라고 생각했지만 보이지 않고,  
	이전에는 flush()를 해야 오류가 안났다 라는 말을 들었으니 내용을 찾아보게 되었음  
  
## flush()를 호출해야 하는가 - 본론  
	java의 api문서를 찾아 들어가봤음  
	URL : https://docs.oracle.com/javase/8/docs/api/java/io/OutputStreamWriter.html##close--  
  
	- 해당 문서 내용  
	==========================================================================  
	public void close()  
			   throws IOException  
	Description copied from class: Writer  
	Closes the stream, flushing it first. Once the stream has been closed, further write() or flush() invocations will cause an IOException to be thrown.  
	Closing a previously closed stream has no effect.  
	...  
	스트림을 닫고 먼저 플러시합니다. 스트림이 닫히고 나면 추가 write() 또는 flush() 호출로 인해 IO 예외가 발생합니다.  
	이전에 닫았던 스트림을 닫으면 아무런 영향을 주지 않습니다.  
	==========================================================================  
  
	변역기를 돌린 내용은 이상하지만 쉽게 설명하면  
	"스트림을 닫기전에, 플러싱을 먼저 한다" 라고 되어 있음  
	참 이 말이 애매한게  
	1. flush()를 내가 해주고 닫으라는건지  
	2. close()를 호출하면 플러싱을 해준다는건지  
	1번인지 2번인지 애매했음 오토라는 단어도 없었으니까  
	작동하는 방식을 보면 2번이 맞을꺼 같았음  
  
	그래서 이 내용에 대해 일단 찾아보니 아래와 같은 내용이 있었음  
	==========================================================================  
	FileWriter의 close() 메서드는 내부적으로 flush() 메서드를 호출합니다.  
	즉, 파일을 닫을 때 flush()를 호출하여 버퍼에 있는 데이터를 실제 파일에 쓰도록 합니다.  
  
	따라서 일반적으로 close()를 호출하면 버퍼가 비워지고 데이터가 파일에 쓰여집니다.  
	따로 flush()를 호출할 필요는 없습니다.  
	FileWriter를 사용할 때 파일을 닫을 때 flush()를 자동으로 호출해주므로 프로그래머가 직접 호출할 필요가 없습니다  
	==========================================================================  
  
	또한 플러시는 스트림을 닫지 않은 상태에서 바이트를 보내고 싶은 경우 사용한다고 보면됨.  
	또한 공식문서에는 버퍼안에 크기가 8192가 넘었고, 스트림을 닫아서는 안되는 경우 flush()를 사용해야 한다고 되어 있음  
  
	그래서 실제로 java에서 해당 코드를 작성해서 디버거 모드로 실행한 결과임  
	- flush() 사용  
	1. FileWriter가 실행될때 파일이 생성되며  
	2. flush()를 호출해도 파일의 용량이 증가하지 않으며  
	3. close()를 호출하면 해당 파일에 용량이 증가함  
	4. 로직 종료  
  
	- flush() 미사용  
	1. FileWriter가 실행될때 파일이 생성되며  
	2. close()를 호출하면 해당 파일에 용량이 증가함  
	3. 로직 종료  
  
## flush()를 호출해야 하는가 - 결론  
	- 쓸내용이 많을경우 중간중간 flush()를 해주자.  
	- 내용이 적을 경우 close()만 호출해도 close()내부에서 flush()를 호출한다  
	- try-with-resources 를쓰면 flush() , close()를 안따져도 되니까 try-with-resources 를 사용하자  
  
	try-with-resources 에 대한 내용은 아래에 정리되어 있음  
		https://github.com/kkimsungchul/study/blob/master/Effective%20Java/%5BEffective%20Java%5D%20%EC%B1%95%ED%84%B09.%20try-finally%EB%B3%B4%EB%8B%A4%EB%8A%94%20try-with-resources%20%EB%A5%BC%20%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC.txt  

{% endraw %}
