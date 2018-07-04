---
layout: post
title: "selector"
date: 2017-6-29
tags: [jQuery,css,选择器]
comments: true
toc: true
---

## css选择器
在css中，选择器是一种模式，用来实现对html页面中的元素实现一对一，一对多或者多对一的控制。

### 基本选择器
1.通用选择器  *

2.class选择器

3.id选择器

4.标签选择器

### 多元素组合选择器
1.E,F 多元素选择器，同时选择两个元素 中间用逗号隔开

2.E F 后代元素选择器，用空格隔开

3.E>F 子元素选择器

4.E+F 毗邻元素选择器

### 属性选择器
1.E[attr] 匹配所有具有attr属性的元素

2.E[att=val] 匹配所有具有attr属性等于val的元素

3.E[att~=val] 匹配所有具有attr属性中含有val的元素

4.E[att\|=val] 匹配所有具有attr属性具有多个连字号分隔的值、其中有一个值以为val开头的元素，主要用于lang属性

### 伪类选择器
1.E:first-child 父元素的第一个子元素

2.E:link 所有未被点击的连接

3.E:visited 所有已被点击的连接

4.E:lactive 正在点击的连接

5.E:hover 鼠标悬停的元素

6.E:focus 获得当前焦点的元素

7.E:enable 匹配表单中激活的元素

8.E:disabled 匹配表单中被禁用的元素

9.E:checked 匹配单选框复选框选中的元素

10.E:selection 匹配当前选中的元素

### 伪元素选择器
1.E:first-line  匹配元素的第一行

2.E:first-letter 匹配元素的第一个字母

3.E:before 在元素之前插入生成的内容

4.E:after 在元素插入之后生成的内容

### 同级元素选择器
1.E~F 匹配任何在E元素之后的同级F元素

### 属性选择器
1.E[att^="val"] 属性attr是以val开头的元素

2.E[att$="val"] 属性attr是以val结尾的元素

3.E[att*="val"] 属性attr是以包含val的元素

### 结构性伪类
1.E:nth-child(n) 匹配父元素的第n个元素 第一个是1

2.E:nth-last-child（n） 匹配父元素的倒数第n个元素 第一个是1


## jQuery选择器

### 简单选择器
1. $("*")  所有元素

2. $("#info") id是info的元素

3. $(".info")  class是info的元素

4. $("p")  所有p标签

5. $(".info1.info2")  class是info1且info2的元素

6. $("p:first")   第一个p元素

7. $("p:last")   最后一个p元素

8. $("tr:even")   所有偶数

9. $("tr:odd")   所有奇数

10. $("li:eq(3)")   第四个li（index从0开始）

11. $("li:gt(3)")   index大于3的元素

12. $("li:lt(3)")   index小于3的元素

13. $("input:not(:empty)")  所有不为空的input元素

14. $(":header")  所有标题元素 即h1-h6

15. $(":animated")  所有动画元素

16. $(":contains('W3School')")  包含指定字符串的所有元素

17. $(":empty")   无子元素或子节点的所有元素

18. $(":hidden")   所有隐藏的p元素

19. $("table:visible")   所有可见的表格

20. $("[href]")  所有带有href属性的元素

21. $("[href=‘#’]")  所有href属性等于#的元素

22. $("[href!=‘#’]")  所有href属性不等于#的元素

23. $("[href$=‘.jpg’]")  所有href属性以.jpg结尾的元素

24. $(":input")  所有input元素

25. $(":text")   所有type=text的input元素

26. $(":password")   所有type=password的input元素

27. $(":radio")   所有type=radio的input元素

28. $(":checkbox")   所有type=checkbox的input元素

29. $(":submit")   所有type=submit的input元素

30. $(":reset")   所有type=reset的input元素

31. $(":button")   所有type=button的input元素

32. $(":image")   所有type=image的input元素

33. $(":file")   所有type=file的input元素

34. $(":enable")  所有激活的input元素

35. $(":disable")  所有禁用的input元素

36. $(":selected")  所有被选取的input元素

37. $(":checked")  所有被选中的input元素

38. $("p.intro") 选取所有 class="intro" 的 <p> 元素

39. $("[href$='.jpg']") 所有带有以 ".jpg" 结尾的属性值的 href 属性

### 组合选择器
1. $("A B") 选取A元素的子孙元素中匹配到B的元素

2. $("A > B") 选择A元素的子元素中匹配到B的元素

3. $("A + B") 选择A元素的下一个兄弟元素中匹配到B的元素

4. $("A ~ B") 选择A元素的后面的兄弟元素中匹配到B的元素

## jquery选取方法
1. first(): 返回对象仅包含选中元素中的第一个元素

2. last(): 返回对象仅包含选中元素中的最后一个元素

3. eq(): 返回的对象只包含指定序号的单个选中元素

4. slice(): 参数为开始和结束序号，返回对象包含从开始到结束序号（不包括结束位置）之间的元素集

5. not(): 返回除了显示排除的元素之外的所有选中元素

6. has(): 返回包含特定后代的元素

7. find(): 返回当前选中元素的子孙元素与选择器相匹配的元素

8. children(): 返回每一个选中元素的直接子元素

9. next(): 返回每一个选中元素的的下一个兄弟元素

10. prev(): 返回每一个选中元素的的上一个兄弟元素

11. silbings(): 返回每一个选中元素的所有兄弟元素

12. parent(): 返回每一个选中元素的父节点

13. parents(): 返回每一个选中元素的祖先节点

