---
layout: post
title: "[Python] IntelliJ 에서 import 한 패키지에서 빨간줄이 뜰 때"
subtitle: "Python IntelliJ 에서 import 한 패키지에서 빨간줄이 뜰 때"
date: 2023-01-01 00:00:00 +0900
categories: Python
---
{% raw %}
## Python - IntelliJ 에서 import 한 패키지에서 빨간줄이 뜰 때  
  
## 아래의 방법대로 진행하면 빨간줄이 사라짐  
	Project Structure  
	-> SDKs  
	-> + 버튼으로 파이썬 SDK 생성  
	-> Add Python SDK  
	-> 가상환경(venv) 또는 시스템 환경인지 정하고 생성  
	-> 생성 된 파이썬 SDK 클릭 후 우측화면에서 Packages 클릭  
	-> + 버튼을 클릭하여 빨간줄이 나오는 패키지 설치                                                                                                                       

{% endraw %}
