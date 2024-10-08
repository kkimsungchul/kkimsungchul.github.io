---
layout: post
title: "[JAVA] CompletableFuture 사용하기"
subtitle: "JAVA CompletableFuture 사용하기"
date: 2023-01-01 00:00:00 +0900
categories: JAVA_Spring
---
{% raw %}
## JAVA - CompletableFuture 사용하기  
  
	공식 문서 : https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html  
  
	참고링크 : https://velog.io/@suyeon-jin/JAVA-CompletableFuture  
		https://www.devkuma.com/docs/java/completable-future/  
  
	테스트 코드 : https://github.com/kkimsungchul/CompletableFutureTest  
  
## CompletableFuture 란  
	JAVA에서 사용할수 있는 비동기 작업 클래스  
		CompletableFuture 리스트의 모든 값이 완료될 때까지 기다릴수 있거나, 아니면 하나의 값만 완료되길 기다릴지 선택할 수 있음  
		CompletableFuture 클래스는 람다 표현식과 파이프라이닝을 활용하면 구조적으로 보기 좋은 코드를 구현할수 있음  
  
	- 특징 및 장점  
		비동기적인 연산:  
			CompletableFuture는 작업의 결과를 기다리지 않고 다른 작업을 수행할 수 있도록 하는 비동기적인 연산을 제공함  
  
		조합 가능한 연산:  
			여러 CompletableFuture 인스턴스를 조합하여 복잡한 연산 흐름을 만들 수 있음.  
			예를 들어, 두 개의 CompletableFuture가 완료되면 어떤 작업을 수행하도록 조합할 수 있음  
			thenCombine, thenCompose, thenAcceptBoth 등의 메서드를 사용하여 여러 작업을 조율할 수 있음  
  
		콜백 지원:  
			작업이 완료되었을 때 콜백을 등록하여 해당 결과를 처리할 수 있음.  
			이를 통해 비동기 작업의 결과에 대한 후속 작업을 정의할 수 있음.  
  
		에러 처리:  
			예외가 발생한 경우 처리할 수 있는 기능을 제공함.  
			exceptionally 메서드를 사용하여 예외를 처리할 수 있음.  
  
		Timeout 및 취소 처리:  
			작업이 일정 시간 내에 완료되지 않을 경우 타임아웃을 설정하거나 작업을 취소할 수 있음.  
  
		비동기 체이닝:  
			thenApply, thenAccept, thenCompose 등의 메서드를 사용하여 여러 비동기 작업을 연결하고 체이닝할 수 있음  
  
		성능 :  
			기존의 thread 코딩에 비해서 core의 성능을 적게 사용함  
  
	- 단점  
		복잡성:  
			CompletableFuture를 사용하면 코드가 더 복잡해질 수 있습니다. 특히 여러 작업을 연결하고 조합하는 경우 코드의 가독성이 감소할 수 있습니다.  
  
		학습 곡선:  
			비동기 및 병렬 프로그래밍의 개념에 익숙하지 않은 경우 CompletableFuture를 사용하는 것은 학습 곡선이 존재할 수 있습니다.  
  
		성능 고려:  
			모든 상황에서 CompletableFuture를 사용하는 것이 항상 성능 향상을 가져오지는 않습니다. 작은 규모의 작업이나 단일 스레드 환경에서는 오히려 오버헤드가 발생할 수 있습니다.  
  
		Debugging의 어려움:  
			비동기 코드의 디버깅은 동기 코드에 비해 어려울 수 있습니다. 스택 트레이스를 추적하는 것이 어려울 수 있습니다.  
  
		CompletableFuture는 비동기 프로그래밍을 쉽게 만들어주는 강력한 도구이지만, 사용 시 주의해야 할 점도 있습니다.  
		코드의 명확성과 유지보수성을 고려하여 적절하게 활용하는 것이 중요합니다.  
  
## 사용 가능 메서드  
	supplyAsync :  
		supplyAsync 메서드는 비동기적으로 작업을 수행하는 CompletableFuture를 생성함  
		작업의 결과를 제공하는 Supplier를 전달하여 사용함  
		==========================================================================  
		CompletableFuture.supplyAsync(() -> "Hello");  
		==========================================================================  
  
	runAsync :  
		runAsync 메서드는 비동기적으로 작업을 수행하는 CompletableFuture를 생성함  
		결과를 반환하지 않는 작업을 실행할 때 사용함  
		==========================================================================  
		CompletableFuture.runAsync(() -> System.out.println("Running asynchronously"));  
		==========================================================================  
  
	thenApply :  
		thenApply 메서드는 이전 작업의 결과를 가지고 특정 함수를 적용하여 새로운 CompletableFuture를 생성함  
		결과를 변환하는데 사용됨  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
		CompletableFuture<Integer> lengthFuture = future.thenApply(String::length);  
		==========================================================================  
  
	thenAccept :  
		thenAccept 메서드는 CompletableFuture에서 비동기 작업이 완료된 후에 결과를 소비하고 추가적인 작업을 수행하는 데 사용함  
		이 메서드는 이전 작업의 결과를 소비하며, 반환 값이 없는 작업을 수행함.  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
		// thenAccept 메서드를 사용하여 작업이 완료된 후에 결과를 소비  
		future.thenAccept(result -> System.out.println("Result: " + result));  
		==========================================================================  
  
	join :  
		join 메서드는 작업이 완료될 때까지 기다리고 결과를 반환함  
		join은 예외를 던지지 않으므로 예외 처리가 필요 없음  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
		String result = future.join();  
		==========================================================================  
  
	get :  
		get 메서드는 작업이 완료될 때까지 기다리고 결과를 반환함  
		하지만 get은 InterruptedException과 ExecutionException을 처리해야 함  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
		String result = future.get();  
		==========================================================================  
  
	exceptionally :  
		exceptionally 메서드는 예외가 발생했을 때 대체 결과를 제공하는 콜백 함수를 등록함  
		예외 처리에 사용됨  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {  
			// 예외 발생 시 대체 결과 반환  
			throw new RuntimeException("Exception occurred!");  
		}).exceptionally(ex -> "Handled Exception: " + ex.getMessage());  
		==========================================================================  
  
	thenCompose :  
		thenCompose 메서드는 두 CompletableFuture를 조합하여 새로운 CompletableFuture를 생성함  
		첫 번째 작업이 완료되면 두 번째 작업을 수행하도록 함  
		==========================================================================  
		CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
		CompletableFuture<String> composedFuture = future.thenCompose(result ->  
			CompletableFuture.supplyAsync(() -> result + " World"));  
		==========================================================================  
  
	thenCombine :  
		두 개의 CompletableFuture의 결과를 조합하여 새로운 결과를 생성함  
		thenCombine은 두 개의 CompletableFuture가 모두 완료된 후에 실행됨  
		==========================================================================  
		CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> 5);  
		CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> 10);  
  
		CompletableFuture<Integer> combinedFuture = future1.thenCombine(future2, (result1, result2) -> result1 + result2);  
		==========================================================================  
  
	allOf :  
		allOf 메서드는 여러 CompletableFuture가 모두 완료될 때까지 기다리는 CompletableFuture를 생성함  
		결과값은 null  
		==========================================================================  
		CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello");  
		CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "World");  
		CompletableFuture<Void> allOfFuture = CompletableFuture.allOf(future1, future2);  
		==========================================================================  
  
	acceptEither :  
		두 개의 CompletableFuture 중 하나가 완료되면 결과를 소비하는 작업을 수행함  
		==========================================================================  
		CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Result from Future 1");  
		CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "Result from Future 2");  
  
		CompletableFuture<Void> eitherFuture = future1.acceptEither(future2, System.out::println);  
		==========================================================================  
  
	runAfterBoth :  
		두 개의 CompletableFuture가 모두 완료된 후에 다른 작업을 실행함  
		결과를 사용하지 않는 작업에 유용함  
		==========================================================================  
		CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Result from Future 1");  
		CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "Result from Future 2");  
  
		CompletableFuture<Void> afterBothFuture = future1.runAfterBoth(future2, () -> System.out.println("Both completed"));  
		==========================================================================  
  
## 예시 코드  
  
	하나의 비동기 작업을 수행 하고  
	여기에 의존적으로 다른 작업을 수행할 수 있도록 해줌  
  
		예제에서 supplyAsync 메서드는 비동기적으로 실행될 작업을 정의하고,  
		thenAccept 메서드는 작업이 완료되면 해당 결과를 콜백으로 받아 출력  
		join 또는 get 메서드를 사용하여 작업이 완료될 때까지 대기함  
		==========================================================================  
		import java.util.concurrent.CompletableFuture;  
		import java.util.concurrent.ExecutionException;  
  
		public class CompletableFutureExample {  
			public static void main(String[] args) throws ExecutionException, InterruptedException {  
				// 비동기적으로 작업을 수행하는 CompletableFuture 생성  
				CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");  
  
				// 작업이 완료되면 콜백 실행  
				future.thenAccept(result -> System.out.println("Result: " + result));  
  
				// 블로킹 없이 결과를 기다림  
				future.join();  
  
				// 또는 작업이 완료될 때까지 블로킹하고 결과를 얻음  
				String result = future.get();  
				System.out.println("Final Result: " + result);  
			}  
		}  
		==========================================================================  
  
	- runAync() 사용  
		해당 메서드는 리턴값이 없는 비동기 메서드  
		==========================================================================  
		CompletableFuture.  
			.runAync(() -> 작업1)  
			.thenRun(() -> 작업1 끝난 후 후속작업2))  
			.thenRun(() -> 후속작업2 끝난 후 후속작업3))  
		==========================================================================  
  
	- supplyAsync() 사용  
		비동기 작업이 다음 작업으로 리턴값을 넘겨줄 수 있음  
  
		thenApply()를 사용하여서 계속 다음 작업으로 값을 넘겨줄수도 있음  
  
		리턴값을 CompletableFuture 타입으로 만들경우 CompletableFuture 을 제거 하고 값을 꺼내야 하기때문에  
		.thenCompose()를 사용해야 함  
  
		exceptionally() 를 사용하여 예외처리를 할수 있음,  
		콜백을 여러개 넣을 필요 없이 마지막에 넣어주면 익셉션 발생시 해당 로직을 타고 내려와 exceptionally()를 실행  
  
		thenApplyAsync() 를 사용하여 스레드풀의 전략에 따라 다른 스레드를 사용할 수있음  
  
		==========================================================================  
		CompletableFuture.  
			.supplyAsync(() -> {  
				작업1  
				return 1;  
			})  
			.thenCompose(s -> {  
				작업2  
				return CompletableFuture.completedFuture(s+1);  
			})  
			.thenApply(s2 -> {  
				작업3  
				return s*3  
			})  
			.exceptionally(e -> -10)  
			.thenAccept(s2 -> System.out.println(s2));  
  
		==========================================================================  
  
		thenApplyAsync() 를 사용하여 스레드풀의 전략에 따라 다른 스레드를 사용할 수있음  
		스레드풀을 직접 적으로 늘려서 사용할 경우 하드웨어 자원이나 환경에 대한 전략을 세워야함  
		==========================================================================  
		ExcutorService es = Exeuctors.newFixedThreadPool(10);  
  
		CompletableFuture.  
			.supplyAsync(() -> {  
				작업1  
				return 1;  
			},es)  
			.thenCompose(s -> {  
				작업2  
				//map 처럼 값을 또 꺼내는게 아니라 값을 담고 있는 CompletableFuture를 리턴할 경우  
				//thenCompose 을 사용  
				return CompletableFuture.completedFuture(s+1);  
			})  
			.thenApplyAsync(s2 -> {  
				작업3  
				return s*3  
			},es)  
			.exceptionally(e -> -10)  
			.thenAcceptAsync(s3 -> System.out.println(s3),es);  
  
		==========================================================================  
  
		위와같이 작업을 진행하게되면, 이전에는 if문이나 try - catch 문 여러개로 예외처리를 진행했던것을  
		간단하게 메서드 체이닝 방식으로도 작성이 가능함  
  

{% endraw %}
