---
layout: post
title: "[JAVA] SimpleDateFormat 과 DateTimeFormatter"
subtitle: "JAVA SimpleDateFormat 과 DateTimeFormatter"
date: 2023-01-01 00:00:00 +0900
categories: JAVA_Spring
---
{% raw %}
## JAVA - SimpleDateFormat 과 DateTimeFormatter  
  
## 참고 링크  
	https://woonys.tistory.com/entry/JavaSimpleDateFormat%EC%9D%84-%EC%93%B0%EB%A9%B4-%EC%95%88%EB%90%9C%EB%8B%A4%EA%B3%A0-featThread-Safe  
	https://stackoverflow.com/questions/57911307/simpledateformat-parsing-issue-for-dateformat-using-milliseconds  
	https://stackoverflow.com/questions/5636491/date-object-simpledateformat-not-parsing-timestamp-string-correctly-in-java-and/6157860##6157860  
  
## 작성하게 된 이유  
	회사에서 DB에저장된 시간을 꺼내와서 화면에 보여줘야할 일이 생김  
	DB에 저장된 시간은 TimeStamp 타입이며 2023-10-13 14:22:32.271816 의 형식으로 저장되어 있음  
  
	로직의 순서는 아래와 같음  
		1. DB에서 조회  
		2. SimpleDateFormat 으로 밀리세컨드 제거  
		3. 화면에 출력  
  
	다만 화면에 출력할 경우, 어떤 데이터는 정상적으로 보이는데 몇몇 데이터들의 시간이 변경되어 있는 것을 확인함  
  
	그래서 디버깅을 시작하면서 데이터를 확인해봄  
		1. DB에서 조회	-> 2023-10-13 14:22:32.271816  
		2. 조회해온 데이터를 서버(JAVA)로 출력 -> 2023-10-13 14:22:32.271816  
		3. SimpleDateFormat 으로 밀리세컨드 제거 -> 2023-10-13 14:27:03  
		4. 화면에 출력 -> 2023-10-13 14:27:03  
  
	3번항목에서 SimpleDateFormat 으로 밀리세컨드를 제거하면서 데이터가 지맘대로 변경된 것을 확인함.  
	해당 내용을 구글에 검색해봤더니 나와같은 문제를 겪은 사람의 내용이 나오며, 거기에 달린 댓글임 (해당 내용은 위의 stackoverflow 참고 링크 확인)  
	=====================================================================  
	You have used .SSSSSS for the fraction of a second whereas the SimpleDateFormat does not support a precision beyond milliseconds (.SSS).  
	It also means that you need to limit the digits in the fraction of a second to three.  
  
	SimpleDateFormat은 밀리초(.SSS)를 초과하는 정밀도를 지원하지 않지만 .SSSSSS를 초 단위로 사용했습니다.  
	그것은 또한 1초 안의 숫자를 3으로 제한해야 한다는 것을 의미합니다.  
	=====================================================================  
  
	이에 대해 분석한 사람의 말에 따르면 SimpleDateFormat에서의 밀리세컨드인 271816 는, 0.271816 초로 계산되는것이 아닌 271816/1000초로 계산이 된다고 함  
	271.816초로 계산되어서, 약 4분 31초가 추가됨  
	아래의 시간을 다시 확인해보면서 비교해보면 4분 31초가 추가된것이 확인됨  
	2023-10-13 14:22:32.271816  
	2023-10-13 14:27:03 -> 위의 시간에서 4분31초가 추가되어있음  
  
	그래서 이문제의 해결방안으로는 아래 두가지 방법을 많이 제안하고 있었음  
	방안 1. DateTimeFormatter 사용  
  
	방안 2. 밀리세컨드는 3자리 까지만 잘라서 사용  
		2023-10-13 14:22:32.271816 -> 2023-10-13 14:22:32.271  
  
	그리고 SimpleDateFormat은 멀티스레드 환경에서 Thread-safe를 보장하지 않는다는 내용도 보이고, java 8에서부터는 DateTimeFormatter을 사용하는것을 권장한다길래 내용을 작성함  
  
## SimpleDateFormat 이란?  
	SimpleDateFormat은 String 타입을 Date 타입으로 파싱 또는 Date 타입을 String으로 포매팅하는데 이를 locale-sensitive 한 방법으로 구현하는 클래스이다.  
	여기서 Locale은 저마다 다르게 관습화된 도메인 규칙(날짜 형식, 화폐 단위 등)을 말하며 Locale-sensitive는 특정 기능을 수행하기 위해 Locale 객체(여기서는 Date가 이에 해당)를 요구하는 동작을 뜻한다.  
	Docs 설명은 아래와 같다.  
	=====================================================================  
	SimpleDateFormat 는 날짜를 파싱 또는 포매팅할 때 사용하는 구현 클래스이다.  
	포매팅(date → text) 및 파싱(text → date), 그리고 정규화를 하는데 쓰인다.  
	=====================================================================  
	※ 출처 https://woonys.tistory.com/entry/JavaSimpleDateFormat%EC%9D%84-%EC%93%B0%EB%A9%B4-%EC%95%88%EB%90%9C%EB%8B%A4%EA%B3%A0-featThread-Safe  
  
	쉽게 말해 SimpleDateFormat은 Java에서 날짜와 시간을 지정된 형식의 문자열로 변환하거나,  
	반대로 지정된 형식의 문자열을 날짜와 시간으로 파싱하는 데 사용되는 클래스이며,java.text 패키지에 속해 있음  
  
	ex) 예제 코드  
	=====================================================================  
	import java.text.SimpleDateFormat;  
	import java.util.Date;  
  
	public class SimpleDateFormatExample {  
		public static void main(String[] args) {  
			Date currentDate = new Date();  
			SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  
			String formattedDate = simpleDateFormat.format(currentDate);  
			System.out.println("Formatted Date: " + formattedDate);  
		}  
	}  
  
	=====================================================================  
  
	SimpleDateFormat 객체를 생성할 때 사용하는 패턴 문자열에 따라 날짜와 시간을 원하는 형식으로 포맷팅할 수 있으며,  
	패턴 문자열에는 년도(y), 월(M), 일(d), 시간(H 또는 h), 분(m), 초(s) 등을 나타내는 문자를 사용할 수 있음.  
  
	또한 문자열을 날짜로 파싱하는데에도 사용되며, 이를 위해서는 parse 메소드를 사용하여 지정된 형식의 문자열을 Date 객체로 변환할 수 있음  
	=====================================================================  
	String dateString = "2023-10-13 14:22:32";  
	SimpleDateFormat dtFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  
	try {  
		Date parsedDate = dtFormat.parse(dateString);  
		System.out.println("Parsed Date: " + parsedDate);  
	} catch (Exception e) {  
		e.printStackTrace();  
	}  
	=====================================================================  
  
## SimpleDateFormat의 주의할점과 단점  
	주의할 점:  
		-쓰레드 안전성 (Thread Safety): SimpleDateFormat은 쓰레드 간 안전하지 않음  
			동시에 여러 쓰레드에서 공유하고 사용할 때 문제가 발생할 수 있으므로 동기화 처리나 쓰레드 로컬 변수를 사용하여 안전하게 사용해야 함  
		- 예외 처리: 날짜 형식이나 파싱 중에 잘못된 형식의 문자열을 사용하면 ParseException이 발생하며, 예외처리가 필요함  
  
	단점:  
		- 복잡한 날짜 형식 지원의 한계: 날짜 및 시간을 특정 형식으로만 포맷팅하거나 파싱할 수 있음, 따라서 복잡한 날짜 형식을 처리하기 어려움.  
		- 제한된 날짜 범위: 날짜를 표현할 수 있는 범위에 제한이 있으며 특히, 기원전(BC, Before Christ)이나 매우 먼 미래의 날짜를 다루기 어려움  
		- 미국식 월-일 순서: 기본적으로 미국식 월-일 순서(MM-dd-yyyy)를 사용하므로, 다른 지역에서는 혼동을 일으킬 수 있음  
  
## SimpleDateFormat의 Thread-unsafe 코드  
	해당 글을 작성한 이유는 아니지만, 추후 멀티 스레드 환경에서 오류가 발생할 여지가 있기에 중요함  
  
	출처 : https://woonys.tistory.com/entry/JavaSimpleDateFormat%EC%9D%84-%EC%93%B0%EB%A9%B4-%EC%95%88%EB%90%9C%EB%8B%A4%EA%B3%A0-featThread-Safe  
  
	=====================================================================  
	import java.text.ParseException;  
	import java.text.SimpleDateFormat;  
	import java.util.Date;  
	import java.util.Locale;  
	import java.util.concurrent.ExecutorService;  
	import java.util.concurrent.Executors;  
  
	public class DateTestThread {  
		private static SimpleDateFormat simpleDateFormat = new SimpleDateFormat("dd-MMM-yyyy HH:mm:ss a", Locale.ENGLISH);  
  
		public static void main(String[] args) {  
			String dateStr = "08-Aug-2023 12:58:47 PM";  
  
			ExecutorService executorService = Executors.newFixedThreadPool(10);  
			Runnable task = new Runnable() {  
				@Override  
				public void run() {  
					parseDate(dateStr);  
				}  
			};  
  
			for (int i = 0; i < 10; i++) {  
				executorService.submit(task);  
			}  
			executorService.shutdown();  
		}  
  
		private static void parseDate(String dateStr) {  
			try {  
				Date date = simpleDateFormat.parse(dateStr);  
				System.out.println("Successfully Parsed Date " + date);  
			} catch (ParseException e) {  
				System.out.println("ParseError " + e.getMessage());  
			} catch (Exception e) {  
				e.printStackTrace();  
			}  
		}  
	}  
	=====================================================================  
  
	출력값  
	=====================================================================  
	java.lang.NumberFormatException: For input string: "1212E"  
		at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)  
		at java.base/java.lang.Long.parseLong(Long.java:711)  
		at java.base/java.lang.Long.parseLong(Long.java:836)  
		at java.base/java.text.DigitList.getLong(DigitList.java:195)  
		at java.base/java.text.DecimalFormat.parse(DecimalFormat.java:2197)  
		at java.base/java.text.SimpleDateFormat.subParse(SimpleDateFormat.java:2244)  
		at java.base/java.text.SimpleDateFormat.parse(SimpleDateFormat.java:1545)  
		at java.base/java.text.DateFormat.parse(DateFormat.java:397)  
		at com.sungchul.date.DateTestThread.parseDate(DateTestThread.java:32)  
		at com.sungchul.date.DateTestThread$1.run(DateTestThread.java:20)  
		at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:539)  
		at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)  
		at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)  
		at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)  
		at java.base/java.lang.Thread.run(Thread.java:833)  
	java.lang.NumberFormatException: For input string: "E.4528023"  
		at java.base/jdk.internal.math.FloatingDecimal.readJavaFormatString(FloatingDecimal.java:2054)  
		...중략...  
	java.lang.NumberFormatException: empty String  
		at java.base/jdk.internal.math.FloatingDecimal.readJavaFormatString(FloatingDecimal.java:1842)  
		...중략...  
	java.lang.NumberFormatException: For input string: "E.4528023E4"  
		at java.base/jdk.internal.math.FloatingDecimal.readJavaFormatString(FloatingDecimal.java:2054)  
		...중략...  
	java.lang.NumberFormatException: For input string: ""  
		at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)  
		...중략...  
	java.lang.NumberFormatException: For input string: ""  
		at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)  
		...중략...  
	Successfully Parsed Date Tue Aug 08 12:58:47 KST 20232023  
	Successfully Parsed Date Tue Aug 08 12:58:47 KST 20232023  
	Successfully Parsed Date Tue Aug 08 12:58:47 KST 2023  
	Successfully Parsed Date Sun Aug 08 12:58:47 KST 1  
  
	Process finished with exit code 0  
	=====================================================================  
  
	위의 예시코드를 실행한 결과값을보면 진짜 개판으로 나오고 Exception도 무지하게 발생한다.  
  
	출처의 내용에 보면 아래와 같이 분석이 되어 있다.  
	=====================================================================  
	이렇게 발생하는 이유는 SimpleDateFormat의 구현 방식에 있다. SimpleDateFormat은 내부적으로 Calender 클래스를 인스턴스화해서 사용한다.  
	----------------------------------------  
	public SimpleDateFormat(String pattern, DateFormatSymbols formatSymbols) {  
			...  
			if (pattern != null && formatSymbols != null) {  
				...  
				this.locale = Locale.getDefault(Category.FORMAT);  
				this.initializeCalendar(this.locale);  
				   ...  
		}  
  
	private void initializeCalendar(Locale loc) {  
			if (this.calendar == null) {  
				assert loc != null;  
  
				this.calendar = Calendar.getInstance(loc);  
			}  
  
		}  
	----------------------------------------  
  
	이때 날짜 포맷팅을 하려고 parse()를 호출하면 내부에서는  
	----------------------------------------  
	calender.clear();  
	calender.add();  
	----------------------------------------  
	가 순차적으로 호출된다.  
	이 상황에서 멀티스레드 간 충돌을 방지해주는 어떤 장치도 없기 때문에 인스턴스에서 충돌이 일어나게 되는 것이다.  
	=====================================================================  
  
## DateTimeFormatter 이란?  
	DateTimeFormatter는 Java 8부터 도입된 클래스로, 날짜와 시간 객체를 특정 형식의 문자열로 포맷팅하거나, 문자열을 날짜와 시간 객체로 파싱하는데 사용  
	이 클래스는 java.time.format 패키지에 포함되어 있음  
  
	ex) 예제 코드  
	=====================================================================  
	import java.time.LocalDateTime;  
	import java.time.format.DateTimeFormatter;  
  
	public class DateTimeFormatterExample {  
		public static void main(String[] args) {  
			// 포맷팅 (날짜 및 시간 객체를 문자열로 변환)  
			LocalDateTime now = LocalDateTime.now();  
			DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");  
			String formattedDateTime = now.format(formatter);  
			System.out.println("Formatted Date and Time: " + formattedDateTime);  
  
			// 파싱 (문자열을 날짜 및 시간 객체로 변환)  
			String dateTimeString = "2023-10-13 14:22:32";  
			LocalDateTime parsedDateTime = LocalDateTime.parse(dateTimeString, formatter);  
			System.out.println("Parsed Date and Time: " + parsedDateTime);  
		}  
	}  
  
	=====================================================================  
  
## DateTimeFormatter의 특징  
	- 포맷팅 (Formatting): DateTimeFormatter를 사용하여 java.time 패키지의 날짜 및 시간 객체를 원하는 형식의 문자열로 변환할 수 있음  
  
	- 파싱 (Parsing): 문자열을 DateTimeFormatter를 사용하여 지정된 형식의 날짜 및 시간 객체로 변환할 수 있음  
  
	- 로케일 지원 (Locale Support): DateTimeFormatter는 로케일을 지원하여 지역화된 날짜 및 시간 형식을 사용할 수 있음  
  
	- 패턴 문자열 사용: 패턴 문자열을 사용하여 날짜, 시간, 밀리초 등을 원하는 형식으로 지정할 수 있음  
		예를 들어, "yyyy-MM-dd HH:mm:ss"는 년도, 월, 일, 시간, 분, 초를 나타내는 패턴임  
  
	- ISO 표준 지원: DateTimeFormatter는 ISO 표준 형식도 지원하여 표준적인 날짜 및 시간 형식을 쉽게 처리할 수 있음  
  
## DateTimeFormatter의 장점  
	- 스레드 안전 (Thread Safety): DateTimeFormatter는 스레드 간에 안전하게 사용할 수 있음  
		멀티스레드 환경에서 여러 스레드가 동시에 DateTimeFormatter를 사용해도 문제가 발생하지 않음  
  
	- 불변성 (Immutability): DateTimeFormatter는 불변 객체이므로 여러 곳에서 안전하게 공유할 수 있음  
  
	- 강력한 날짜 및 시간 형식 지원: 다양한 날짜 및 시간 형식을 지원하며, 패턴을 사용하여 사용자 정의 형식을 지정할 수 있음, 이로 인해 다양한 형식의 날짜와 시간을 다룰 수 있음  
  
	- 로케일 지원 (Locale Support): 로케일에 따라 다른 지역화된 형식을 지원할 수 있음, 지역화된 날짜 및 시간 형식을 생성할 때 유용함  
  
	- ISO 표준 준수: DateTimeFormatter는 ISO 표준 형식을 준수하여 표준적인 날짜 및 시간 형식을 쉽게 처리할 수 있음  
  
	- 년, 월, 일 등을 안전하게 추출: DateTimeFormatter를 사용하여 년, 월, 일, 시간, 분, 초 등 각각의 구성 요소를 안전하게 추출할 수 있음  
  
## DateTimeFormatter의 단점  
  
	- 복잡성: DateTimeFormatter는 더 복잡한 API이므로 처음 사용하는 개발자에게는 SimpleDateFormat보다 사용하기가 어려울 수 있으며, 패턴의 이해와 사용법을 숙지해야 함  
  
	- 성능: SimpleDateFormat보다 성능 면에서 약간의 오버헤드가 발생할 수 있으나 대부분의 경우에는 이 오버헤드가 무시될 정도로 작음  
  
	- DateTimeFormatter는 스레드 안전성과 강력한 형식 지원으로 인해 주로 권장되며, Java 8 이상에서는 기본적으로 사용하는 것이 좋음  
  
## SimpleDateFormat 과 DateTimeFormatter로 작성한 메소드 비교  
  
	SimpleDateFormat으로 작성한 메소드  
	=====================================================================  
    /**  
     * 입력받은 날짜와 포맷을, 원하는 포맷으로 변경  
     * @param date 입력받은 날짜  
	 * @param nowFormat 입력받은 날짜의 데이터 포맷  
	 * @param changeFormat 변경할 데이터 포맷  
     * @return String 변경된 데이터 포맷으로 리턴  
     * */  
	public static String changeDateFormat(String date , String nowFormat , String changeFormat) {  
		SimpleDateFormat dtFormat = new SimpleDateFormat(nowFormat);  
		SimpleDateFormat newDtFormat = new SimpleDateFormat(changeFormat);  
		Date formatDate = null;  
		try {  
			formatDate = dtFormat.parse(date);  
		} catch (Exception e) {  
			e.printStackTrace();  
		}  
		return newDtFormat.format(formatDate);  
	}  
	=====================================================================  
  
	DateTimeFormatter로 작성한 메소드  
	=====================================================================  
    /**  
     * 입력받은 날짜와 포맷을, 원하는 포맷으로 변경  
     * @param date 입력받은 날짜  
	 * @param nowFormat 입력받은 날짜의 데이터 포맷  
	 * @param changeFormat 변경할 데이터 포맷  
     * @return String 변경된 데이터 포맷으로 리턴  
     * */  
	public static String changeDateFormat(String date, String nowFormat, String changeFormat) {  
		DateTimeFormatter dtFormatter = DateTimeFormatter.ofPattern(nowFormat);  
		LocalDateTime dateTime = LocalDateTime.parse(date, dtFormatter);  
  
		DateTimeFormatter newDtFormatter = DateTimeFormatter.ofPattern(changeFormat);  
		return dateTime.format(newDtFormatter);  
	}  
	=====================================================================  
  
	코드의 양도 간결해지며, 시간이 달라질 오류가 발생하지 않음  

{% endraw %}
