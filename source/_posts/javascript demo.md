---
layout: post
title: "常用代码片段"
date: 2017-07-3
tags: [javaScript,jQuery]
comments: true
toc: true
---

## 常用js代码片段整理

收集了一些在项目中遇见的一些实用代码片段，以便加快开发速度，提高工作效率。将持续更新

### 全选操作

```javascript

   function checkAll (){
       var selectall = documnent.getElementById("selectall");
       var allbox = document.getElementsByName("allbox");
       if(selectall.checked){
           for(var i = 0; i < allbox.length; i++){
               allbox[i].checked = true;
           }
       }else{
           for(var i = 0; i < allbox.length; i++){
               allbox[i].checked = false;
           }
       }
   }

```

### tab效果
```javascript

    function Change(btn, active, div) {
        $(btn).mouseover(function() {
            $(this).addClass(active).siblings('li').removeClass(active);
            $(div).eq($(this).index()).css('display', 'block').siblings().css('display', 'none')
        });
    }

```

### 禁止页面右键菜单

```javascript
    $(docunment).ready(function() {
        $(document).bind("contextmenu",function(e) {
            return false;
        });
    });

```

### 新窗口打开页面

```javascript
    $(document).ready(function() {
        $("a[href^='http://']").attr("target","_blank");

        $("a[rel $= 'extrenal']").click(function() {
            this.target = "_blank";
        });
    })

```

### 返回顶部
```javascript
    jQuery.fn.scrollTo = function (speed) {
        var targetOffset = $(this).offset().top;
        $('html body').stop().animate({scrollTop:targetOffset},speed);
        return this;
    }

    //use
    $("goToHeader").click(function (){
        $("body").scrollTo(500);
        return false;
    })

```

### 获取鼠标位置
```javascript
    $(docunment).ready(function (){
        $(document).mousemove(function (e){
            $("div").html("X："+e.pageX + "| Y" + e.pageY);
        });
    });

```

### 判断div在屏幕中央
```javascript
    jQuery.fn.center = function(){
        this.css("position","absolute");
        this.css("top",($(window).height() - this.height()) / 2+$(window).scrollTop()+"px");
        this.css("left",($(window).width() - this.width()) / 2+$(window).scrollLeft()+"px");
        return false;
    }
    //use
    $("div").center();

```
### 关闭所有动画
```javascript
    $(document).ready(function(){
        jQuery.fx.off = true;
    });
```

### 设置全局ajax参数
```javascript
    $("#load").ajaxStart(function(){
        showLoading();  // 显示loading
        disableBUttons();   // 禁用按钮
    });

    $("#load").ajaxComplete(function (){
        hideLoading();  
        enableBUttons();   
    })

```
### 获取选中的下拉框
```javascript
    $("#ele").find("option:selected");

    $("#ele option:selected");
```

### 综合动画
```javascript
    $(function(){
        $(".myImg").css("opacity","0.5");
        $(".myInmg").click(function(){
            $(this).animate({left:"400px",height:"200px",opacity:"1"},3000)
                    .animate({top:"200px",width:"200px"},3000)
                    .fadeOut("slow");
        })

    });
```
### 给页面中的每个p标签加事件
```javascript
    var items = document.getElementsByTagName("p");
    for (var i = 0; i < items.length; i++){
        items[i].onclick = function(){
            //do something
        }
    }

```

### 判断复选框是否选中
```javascript
    jQuery(document).ready(function($){
        var $checkbox = $("#checkbox");
        $checkbox.click(function(){
            if($checkbox.is(":checked")){
                alert("已选中");
            }
        })
    })

```
### 设置cookie

```javascript
    //添加cookie
 function addCookie(key, value, day) {
        var date = new Date();
        date.setDate(date.getDate() + day);
        document.cookie = key + '=' + encodeURI(value) + ';expires=' + date;
    }

	 //获取cookie
	 function getCookie(key) {
	     var str = decodeURI(document.cookie);
	     var arr = str.split('; ');
	     for (var i = 0; i < arr.length; i++) {
	         var arr1 = arr[i].split('=');
            if (key == arr1[0]) {
                return arr1[1];
            }
        }
    }
 	//删除cookie
 	function delCookie(key, value) {
	 	addCookie(key, value, -1)
    }
```
### 返回顶部
```javascript
    function backToTop(btnId){
        var btn = document.getElementById(btnId);
        var d = document.documentElement;
        var b = document.body;
        window.onscroll = set;
        btn.style.display = "none";
        btn.onclick = function(){
            btn.style.display = "none";
            window.onscroll = null;
            this.timer = setInterval(function(){
                d.scrollTop -= Math.ceil((d.scrollTop+b.scrollTop)*0.1);
                b.scrollTop -= Math.ceil((d.scrollTop+b.scrollTop)*0.1);
                if((d.scrollTop+b.scrollTop) == 0){
                    clearInterval(btn.timer,window.onscroll = set);
                }
            },10);

        };
        function set(){
            btn.style.display = (d.scrollTop+b.scrollTop>100)?"block":"none";
        }
    }
    //use
    backToTop('goTop');

```

### 获取复选框的值

```javascript
    function get_checkbox_value(field){
        if(field$$field.length){
            for(var i = 0; i < field.length; i++){
                if(field[i].checked && !field[i].disable){
                    return field[i].value;
                }
            }
        }else{
            return ;
        }
    }


```

### 窗体改变事件
```javascript
    (function(){
        var fn = function(){
            var w = document.documentElement ? document.documentElement.clientWidth : documnent.body.clientWidth,
                r = 1255,
                b = Element.extend(document.body),
                classname = b.className;
            if(w < r){
                //当窗体的宽度小于1255的时候执行相应操作
            }else{
                //当窗体的宽度大于1255的时候执行相应操作
            }
        }
        if(window.addEventListener){
            window.addEventListener('resize',function(){
                fn();
            })
        }else if(window.attachEvent){
            window.addEventListener('onresize',function(){
                fn();
            }) 
        }
        fn();
    })();

```

