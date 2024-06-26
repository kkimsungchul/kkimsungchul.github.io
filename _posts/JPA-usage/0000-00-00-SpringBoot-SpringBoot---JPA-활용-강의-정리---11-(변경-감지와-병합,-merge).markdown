---
layout: post
title: "[SpringBoot] SpringBoot - JPA 활용 강의 정리 - 11 (변경 감지와 병합, merge)"
subtitle: "SpringBoot SpringBoot - JPA 활용 강의 정리 - 11 (변경 감지와 병합, merge)"
date: 2023-01-01 00:00:00 +0900
categories: JPA-usage
---
{% raw %}
## SpringBoot - JPA 활용 강의 정리 - 11 (변경 감지와 병합,merge)  
  
## 준영속 엔티티  
	영속성 컨텍스트가 더는 관리하지 않는 엔티티를 말함.  
	강의에서는 itemService.saveItem(book)에서 수정을 시도하는 Book 객체임  
	Book 객체는 이미 DB에 한번 저장되어서 식별자가 존재함  
	이렇게 임의로 만들어낸 엔티티도 기존 식별자를 가지고 있으면 준영속 엔티티로 볼수 있음  
  
## 준영속 엔티티를 수정하는 두가지 방법  
	- 변경 감지 기능 사용  
	- 병합(merge) 사용  
  
## 변경 감지 기능 사용(dirty checking)  
	- 영속성 컨텍스트에서 엔티티를 다시 조회한 후에 데이터를 수정하는 방법  
	- 트랜잭션 안에서 엔티티를 다시 조회, 변경할 값 선택 -> 트랜잭션 머킷 시점에 변경 감지 (Dirty Checking)이 동작하여 데이터베이스에 Update SQL 실행  
	==========================================================================  
    @Transactional  
    public void updateItem(Long itemId, Book bookParam){  
        //findItem 으로 찾아온 객체는 영속 상태임  
        Item findItem = itemRepository.findOne(itemId);  
        findItem.setPrice(bookParam.getPrice());  
        findItem.setName(bookParam.getName());  
        findItem.setStockQuantity(bookParam.getStockQuantity());  
  
        //스프링의 @Transactional 어노테이션에 의하여 자동 커밋  
    }  
	==========================================================================  
  
## 병합(merge) 사용  
	기존에 넘어온 객체는 영속성 컨텍스트에서 관리하지 않고, merge 하고 리턴받은 객체는 영속성 컨텍스트에서 관리함  
	- 병합은 준영속 상태의 엔티티를 영속 상태로 변경할 때 사용하는 기능.  
  
	- 예시 1  
	==========================================================================  
    @Transactional  
    public void updateItem(Item itemParam){  
		Item mergeItem = em.merge(item);  
    }  
	==========================================================================  
  
	- 예시 2  
		예시 1에서 넘겨 받는 mergeItem은 아래의 코드를 실행한 내용의 return 값을 같음  
	==========================================================================  
	@Transactional  
    public Item updateItem(Long itemId, Book bookParam){  
        Item findItem = itemRepository.findOne(itemId);  
        findItem.setPrice(bookParam.getPrice());  
        findItem.setName(bookParam.getName());  
        findItem.setStockQuantity(bookParam.getStockQuantity());  
        return findItem;  
    }  
	==========================================================================  
	- 병합 동작 방식  
		1. merge() 를 실행  
		2. 파라미터로 넘어온 준영속 엔티티의 식별자 값으로 1차 캐시에서 엔티티를 조회  
			2-1. 만약 1차 캐시에 엔티티가 없으면 데이터베이스에서 엔티티를 조회하고 1차 캐시에 저장  
		3. 조회한 영속 엔티티에 넘겨받은 엔티티의 값을 채워넣음  
		4. 영속상태인 객체를 반환  
  
	- 병합시 동작 방식을 간단히 정리  
		1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회.  
		2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체(병합)  
		3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에서 UPDATE SQL 이 실행  
  
	※ 주의사항  
		변경 감지 기능을 사용하면 원하는 속성만 선택해서 변경할 수 있지만, 병합을 사용하면 모든 속성이 변경됨  
		병합시 값이 없으면 'null' 로 업데이트 할 위험도 있음 (병합은 모든 필드를 교체함)  
  
		merge는 식별자 값을 가지고 있는 새로운 객체로 전부다 업데이트 함  
		Dirty Checking은 변경된 값만 업데이트 함  
  
	※ 실무에서는 merge 보다는 변경감지(dirty checking)를 사용하도록 해야함  
	※ changePrice() , changeName()등의 메서드를 만들어서 해당 메서드 안에서 엔티티를 변경하도록 개발  
  
	- 컨트롤러에서 어설프게 엔티티를 생성하지 말것  
		주석부분이 엔티티를 만들려고 한것이며, 주석안친 코드가 개선된 코드  
	==========================================================================  
    @PostMapping("/items/{itemId}/edit")  
    public String updateItem(@PathVariable("itemId") Long itemId, @ModelAttribute("form") BookForm form){  
//        Book book = new Book();  
//        book.setId(form.getId());  
//        book.setName(form.getName());  
//        book.setPrice(form.getPrice());  
//        book.setStockQuantity(form.getStockQuantity());  
//        book.setAuthor(form.getAuthor());  
//        book.setIsbn(form.getIsbn());  
//  
//  
//        itemService.saveItem(book);  
        itemService.updateItem(itemId , form.getName() , form.getPrice(), form.getStockQuantity());  
        return "redirect:/items";  
    }  
	==========================================================================  
	- 트랜잭션이 있는 서비스 계층에 식별자(id)와 변경할 데이터를 명확하게 전달할것 (파라미터 or DTO)  
  
	- 트랜잭션이 있는 서비스 계층에서 영속 상태의 엔티티를 조회하고, 엔티티의 데이터를 직접 변경할 것  
  
	- 트랜잭션 커밋 시점에 변경 감지가 실행됨  
  
	- setter 사용하지말고 entity 에서 추적할수 있도록 개발할 것  

{% endraw %}
