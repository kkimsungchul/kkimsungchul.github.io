---  
layout: post  
title: "[JavaScript] jqtree 사용"  
subtitle: "JavaScript jqtree 사용"  
date: 0000-00-00 00:00:00 +0900  
categories: JSP&javascript  
---  
{% raw %}  
## jqTree  
  
※ 문제발생  
	jqtree 랑 jquery-ui랑 혼용불가  
※ 해결  
	jQuery.noConflict(true); 를 선언하여서 사용  
	* 참고 문서 : " [JavaScript] jquery_다른버전 두개 동시에 사용하기.txt "  
  
## 참고 URL  
	https://m.blog.naver.com/PostView.nhn?blogId=galma73&logNo=80198151598&proxyReferer=https%3A%2F%2Fwww.google.com%2F  
	https://www.jqueryscript.net/demo/jQuery-Plugin-for-Tree-Widget-jqTree/##demo  
	http://mbraak.github.io/jqTree/  
  
## 선언  
  
	<script src="/common/bootstrap/js/tree/tree.jquery.js"></script>  
	<link rel="stylesheet" href="/common/bootstrap/css/tree/jqtree.css">  
  
## 데이터 형식  
	* JSON 형식의 데이터를 사용하며 데이터형식은 아래와 같음  
	* 하위노드를 표시할때는 children : [] 안에 데이터를 넣어서 표시  
	* JSON 트리구조로 표현하는 방법은 " [JavaScript] JSON 트리구조 변환 (Json tree, jqtree).txt " 파일 참고  
  
	ex1)  
		var data = [  
			{  
				name: 'node1', id: 1,  
				children: [  
					{ name: 'child1', id: 2 },  
					{ name: 'child2', id: 3 }  
				]  
			},  
			{  
				name: 'node2', id: 4,  
				children: [  
					{ name: 'child3', id: 5 }  
				]  
			}  
		];  
  
	ex) [{  
            dept_par_id: "0",  
            id: "100000",  
            label: "코스콤",  
            dept_level: 0,  
        children: [{  
                dept_par_id: "100000",  
                id: "110000",  
                label: "비서실",  
                dept_level: 1,  
            },  
                {  
                    dept_par_id: "100000",  
                    id: "120000",  
                    label: "IT인프라부",  
                    dept_level: 1,  
                },  
                {  
                    "dept_par_id": "100000",  
                    "id": "130000",  
                    "label": "시장본부",  
                    "dept_level": 1,  
                    children: [{  
                        "dept_par_id": "130000",  
                        "id": "131000",  
                        "label": "해외개발TF팀",  
                        "dept_level": 2,  
                    }]  
                },  
                {  
                    "dept_par_id": "130000",  
                    "id": "132000",  
                    "label": "시장전략부",  
                    "dept_level": 2,  
                    children: [{  
                        "dept_par_id": "132000",  
                        "id": "132100",  
                        "label": "시장컨설팅팀",  
                        "dept_level": 3,  
                    },  
                        {  
                            "dept_par_id": "132000",  
                            "id": "132200",  
                            "label": "시장분석팀",  
                            "dept_level": 3,  
                        }]  
                },  
                {  
                    "dept_par_id":  
                        "100000",  
                    "id":  
                        "140000",  
                    "label":  
                        "경영전략본부",  
                    "dept_level":  
                        1,  
                    children:  
                        [{  
                            "dept_par_id": "140000",  
                            "id": "141000",  
                            "label": "구매업무실",  
                            "dept_level": 2,  
                        }  
                        ]  
                }  
                ,  
                {  
                    "dept_par_id":  
                        "100000",  
                    "id":  
                        "150000",  
                    "label":  
                        "기술연구소",  
                    "dept_level":  
                        1,  
                },  
            ]  
    }];  
  
## 사용  
  
		$("##deptTree").tree({  
			data: data,  
			selectable: true,  
			useContextMenu: true,  
			dragAndDrop: true  
		})  
  
## ex 01  
  
     /*        var deptData = "";  
                $.ajax({  
                    type: 'post',  
                    url: "/dept/getDeptTree.do",  
                    success: function (data) {  
                        deptData = data;  
                        console.log(deptData);  
                        $('##deptTree').tree({  
                            data: deptData,  
                            selectable: true,  
                            useContextMenu: true,  
                            /!*클릭 이벤트*!/  
                            onCanSelectNode: function (node) {  
                                console.log("node.children : ", node.children);  
                                getDeptTree(node);  
                                getUserList(1, node.id);  
                            },  
                            dragAndDrop: true  
                        });  
                    }  
  
                });*/  
        var deptData = [  
            {  
                "dept_par_id": "0",  
                "id": "100000",  
                "label": "무역회사",  
                "dept_level": 0,  
                children: [{  
                    "dept_par_id": "100000",  
                    "id": "110000",  
                    "label": "비서실",  
                    "dept_level": 1,  
                }, {  
                    "dept_par_id": "100000",  
                    "id": "120000",  
                    "label": "IT인프라부",  
                    "dept_level": 1,  
                }, {  
                    "dept_par_id": "100000",  
                    "id": "130000",  
                    "label": "시장본부",  
                    "dept_level": 1,  
                    children: [{  
                        "dept_par_id": "130000",  
                        "id": "131000",  
                        "label": "해외개발TF팀",  
                        "dept_level": 2,  
                    }, {  
                        "dept_par_id": "130000",  
                        "id": "132000",  
                        "label": "시장전략부",  
                        "dept_level": 2,  
                        children: [{  
                            "dept_par_id": "132000",  
                            "id": "132100",  
                            "label": "시장컨설팅팀",  
                            "dept_level": 3,  
                        }, {  
                            "dept_par_id": "132000",  
                            "id": "132200",  
                            "label": "시장분석팀",  
                            "dept_level": 3,  
                        }]  
                    }]  
                }, {  
                    "dept_par_id": "100000",  
                    "id": "140000",  
                    "label": "경영전략본부",  
                    "dept_level": 1,  
                    children: [{  
                        "dept_par_id": "140000",  
                        "id": "141000",  
                        "label": "구매업무실",  
                        "dept_level": 2,  
                    }]  
                }, {  
                    "dept_par_id": "100000",  
                    "id": "150000",  
                    "label": "기술연구소",  
                    "dept_level": 1,  
                }]  
            }  
        ];  
  
        /*        $('##deptTree').tree({  
                    data: deptData,  
                    selectable: true,  
                    useContextMenu: true,  
                    /!*클릭 이벤트*!/  
  
                    onCanSelectNode: function (node) {  
  
                        getDeptTree(node);  
                        getDeptList(node);  
                    },  
                    dragAndDrop: true,  
                    autoOpen: true  
                });*/  
        /*$("##deptTree").load("tree_sample/tree_sample.html");*/                                                                                                                                                                                                                                                                                                  
{% endraw %}