---
layout: post
title: "[Vue] eslint 끄기"
subtitle: "Vue eslint 끄기"
date: 2022-11-16 05:41:33 +0900
categories: Vue
---
[ Vue - eslint 끄기 ]

	해당 옵션이 켜저있으면 이것저것 개짱남
	끄자

# vue.config.js 파일 수정

	아래와 같이   lintOnSave: false 옵션을 추가
	=================================================================================================================
	const { defineConfig } = require('@vue/cli-service')
	module.exports = defineConfig({
	  transpileDependencies: true,
	  lintOnSave: false

	})
	=================================================================================================================
