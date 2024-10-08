---
layout: post
title: "[JavaScript] FullCalendar 사용하기(풀캘린더)"
subtitle: "JavaScript FullCalendar 사용하기(풀캘린더)"
date: 2023-01-01 00:00:00 +0900
categories: JSP_javascript
---
{% raw %}
## FullCalendar 사용하기(풀캘린더)  
	공식 사이트 : https://fullcalendar.io/  
  
## 사용 코드  
	=====================================================================  
	<!DOCTYPE html>  
	<html lang='ko'>  
	  <head>  
		<meta charset='utf-8' />  
		<script src='https://cdn.jsdelivr.net/npm/fullcalendar@6.1.11/index.global.min.js'></script>  
		<script>  
  
			//캘린더에 표시할 데이터를 담을 변수 초기화  
			var calendarData = []  
  
			document.addEventListener('DOMContentLoaded', function() {  
				var calendarEl = document.getElementById('calendar');  
  
				var calendar = new FullCalendar.Calendar(calendarEl, {  
					initialView: 'dayGridMonth',  
					timeZone: 'local',  
					events: calendarData,  
					eventClick: function(info) {  
						// 클릭한 이벤트에 대한 정보를 처리하는 함수  
						console.log('클릭한 이벤트의 제목:', info.event.title);  
						console.log('클릭한 이벤트의 시작 날짜:', info.event.start);  
						console.log('클릭한 이벤트의 종료 날짜:', info.event.end);  
  
					}  
				});  
  
				calendar.render();  
  
				//캘린더 그린 후 바로 호출되는 함수  
				//캘린더 초기 데이터는 [] 로 아무것도 없기 때문에 바로 호출해야 함  
				getCalendarData(calendar.getDate().getFullYear() , calendar.getDate().getMonth()+1)  
  
				// 이전 달 버튼 클릭 시  
				document.querySelector('.fc-prev-button').addEventListener('click', function() {  
					var prevDate = calendar.getDate();  
					getCalendarData(prevDate.getFullYear() , prevDate.getMonth() + 1); // getMonth()는 0부터 시작하므로 +1  
					//getYear()을 하면 올해년도 - 1900을 한 데이터를 가져와서 사용하면 안됨,  
					//getYear() 사용시 2024년이면 124가 나옴  
				});  
  
				// 다음 달 버튼 클릭 시  
				document.querySelector('.fc-next-button').addEventListener('click', function() {  
					var nextDate = calendar.getDate();  
					getCalendarData(nextDate.getFullYear() , nextDate.getMonth() + 1); // getMonth()는 0부터 시작하므로 +1  
				});  
  
				//공지사항 데이터 가져오는 함수  
				function getCalendarData(year , month){  
					//여기서 비동기 통신으로 서버에 데이터를 요청  
					//api/notice/{startDate}/{endDate}  
  
					//month 변수를 넘겨받으면 1 2 3 4 5 6 7 8 9 10 11 12 로 되어있어서 10보다 작을경우 앞에 0을 붙여줌  
					month = (month < 10 ? '0' : '') + month;  
  
					//noticeFlag 는 임시로 작성한 변수명임, 이건 현식대리한테 변수명 뭘로할껀지 물어보면됌  
					//2월일때만 true로 주도록 했음  
					var noticeFlag = (month == '02' ? true : false);  
  
					console.log("이동한 년은 : " + year );  
					console.log("이동한 달은 : " + month );  
					calendarData = [  
						{  
							title : year + "_" + month + "공지1",  
							start: year + "-" + month + "-01",  
							end: year + "-" + month + "-02",  
							display: 'background',  
							newNoticeFlag: false  
						},  
						{  
							title : year + "_" + month + "공지2",  
							start: year + "-" + month + "-05",  
							end: year + "-" + month + "-06",  
							display: 'background',  
							newNoticeFlag: false  
						},  
						{  
							title : year + "_" + month + "공지3",  
							start: year + "-" + month + "-08",  
							end: year + "-" + month + "-10",  
							display: 'background',  
							newNoticeFlag: false  
						},  
						{  
							title : year + "_" + month + "공지4",  
							start: year + "-" + month + "-20",  
							end: year + "-" + month + "-23",  
							display: 'background',  
							newNoticeFlag: false  
						},  
						{  
							title : year + "_" + month + "공지5",  
							start: year + "-" + month + "-25",  
							end: year + "-" + month + "-25",  
							display: 'background',  
							newNoticeFlag: noticeFlag  
						}  
					];  
					//최신 공지사항 찾기  
					//요청한 데이터중 최신 공지 값이 있으면 background 값을 red로 변경하게 했음.  
					calendarData.forEach(function(item) {  
						if (item.newNoticeFlag) {  
							item.backgroundColor = 'red';  
						}  
					});  
					console.log(calendarData);  
  
					//캘린더에 있는 데이터 삭제  
					//calendar.removeAllEventSources();  
  
					//캘린더에 데이터 추가  
					calendar.addEventSource(calendarData);  
					//calendar.refetchEvents();  
				}  
  
			});  
  
			//캘린더 그리기  
			/*  
			document.addEventListener('DOMContentLoaded', function() {  
				var calendarEl = document.getElementById('calendar');  
				var calendar = new FullCalendar.Calendar(calendarEl, {  
					initialView: 'dayGridMonth',  
					timeZone: 'local',  
					//initialDate: '2024-02-27',  
					events: calendarData,  
					datesSet: function(info) {  
						var prevMonth = info.start.getMonth() + 1; // 이전 달의 월  
						var nextMonth = info.end.getMonth() + 1; // 다음 달의 월  
						console.log('이전 달:', prevMonth);  
						console.log('다음 달:', nextMonth);  
						//이전달 클릭 이벤트  
						document.querySelector('.fc-prev-button').addEventListener('click', function() {  
							getCalendarData(prevMonth);  
						});  
  
						//다음달 클릭 이벤트  
						document.querySelector('.fc-next-button').addEventListener('click', function() {  
							getCalendarData(nextMonth);  
						});  
					}  
				});  
				calendar.render();  
			})  
			*/  
  
		</script>  
	  </head>  
	  <body>  
		<div id='calendar'></div>  
	  </body>  
	</html>  
  
	=====================================================================  

{% endraw %}
