---
layout: post
title: "[Spring Boot] JSON데이터 변환하기"
subtitle: "Spring Boot JSON데이터 변환하기"
date: 2023-01-01 00:00:00 +0900
categories: SpringBoot
---
{% raw %}
## TypeReference  
  
TypeReference 얘가 뭐하는건데?  
ChatGPT  
TypeReference는 Jackson 라이브러리에서 제공하는 유틸리티 클래스로, Java의 제네릭 타입을 사용하는 객체의 타입 정보를 보존하기 위해 사용됩니다.  
  
Jackson은 JSON 데이터를 Java 객체로 역직렬화할 때 사용됩니다. 그러나 Java에서 제네릭 타입 정보는 컴파일 시점에만 사용되며 런타임에는 소거되므로, Jackson이 제네릭 타입을 올바르게 처리하려면 TypeReference를 사용하여 타입 정보를 제공해야 합니다.  
  
TypeReference를 사용하여 JSON 데이터를 Java 객체로 변환할 때 Jackson은 제네릭 타입을 올바르게 유지하면서 JSON 데이터를 역직렬화합니다.  
  
예를 들어, Map<String, Object>을 역직렬화하려면 다음과 같이 TypeReference를 사용할 수 있습니다:  
  
java  
Copy code  
import com.fasterxml.jackson.core.type.TypeReference;  
  
TypeReference<Map<String, Object>> typeReference = new TypeReference<Map<String, Object>>() {};  
Map<String, Object> resultMap = objectMapper.readValue(jsonString, typeReference);  
TypeReference의 인스턴스를 생성할 때 익명 클래스를 사용하여 제네릭 타입 정보를 보존하고, 이 정보를 readValue 메서드에 전달하여 올바른 타입으로 역직렬화할 수 있습니다.  

{% endraw %}
