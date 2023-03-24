---
layout: post
title: "[Spring Boot] 테스트를 위한 테이블 생성 쿼리문"
subtitle: "Spring Boot 테스트를 위한 테이블 생성 쿼리문"
date: 2023-03-24 12:40:26 +0900
categories: SpringBoot
---
CREATE TABLE `board` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`writer` VARCHAR(10) NULL DEFAULT NULL,
	`title` VARCHAR(100) NULL DEFAULT NULL,
	`content` VARCHAR(500) NULL DEFAULT NULL,
	`created_date` TIMESTAMP NULL DEFAULT NULL,
	`modified_date` TIMESTAMP NULL DEFAULT NULL,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB
AUTO_INCREMENT=2
;
