---
date: 2015-11-31T00:30:03+08:00
description: ""
tags: ["Hugo","静态网站生成器","博客","教程","网站"]
title: "Hugo静态网站生成器中文教程"
topics: []
draft: false
url: /post/2015-11-31
---


Markdown语法    


#1、标题：
两种标题的语法，Setext 和 atx 形式（共有六级）：
第一种：Setext 形式是用底线的形式，通过在文字下方添加“=”和“-”，他们分别表示一级标题和二级标题。  
一级标题
===
二级标题
----

第二种：Atx 形式，在文字开头加上 “#”，通过“#”数量表示几级标题。（一共只有1~6级标题，1级标题字体最大）  
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题




#2、列表：  
有序列表：使用数字后面跟上句号。（还要有空格）  
   1. 有序一  
   2. 有序二  
   3. 有序三  
   
注意：在“1.”与文本之间有空格；  
   
无序列表：在文字开头添加(*, +, and -)实现无序列表。但是要注意在(*, +, and -)和文字之间需要添加空格。（建议：一个文档中只是用一种无序列表的表示方式）  
   - 无序一  
   - 无序二  
   - 无序三  

   * 无序一
   * 无序二
   * 无序三

   + Red
   + Green
   + Blue  
   
注意：在“-”或“*”与文本之间有空格；




#3、引用：  
区块引言可以有级别（例如：引言内的引言），只要根据级别加上不同数量的 >   
> 引用  
>> 引用  
>>>> 引用  

注意： 符号“>”与文本之间需要有空格；




#4、字体强调：  
星号（*）和底线（_）作为标记强调字词的符号  
斜体：将需要设置为斜体的文字两端使用1个“ * ”或者“ _ ”夹起来  
   *这是斜体字*    
   _这也是斜体字_  
   
粗体：将需要设置为斜体的文字两端使用2个“ * ”或者“ _ ”夹起来  
   **这是粗体字**  
   __这也是粗体字__  
注意： 在“*”或“**”与文本之间是没有空格的；  




#5、代码：两种方式：
第一种：简单文字出现一个代码框，使用“`”字符。（`不是单引号而是左上角的ESC下面~中的`）  
行内代码：  
   这里是行内代码 `int a = 123456;`;  
独立代码段：  

   ```cpp
   #include <stdio.h>
   
   int main(int argc, char** argv)
   {
       int a = 12345;
       int i = 0；
       for(i = 10; i >= 0; i--)
           a = a + i;
           
       if(a >= 20)
           printf("a= [%d]\n", a);
           
        return 0;
   }
   ```


第二种：大片文字需要实现代码框。使用Tab或四个空格。    

    #include <stdio.h>
       
    int main(int argc, char** argv)
    {
       int a = 12345;
       int i = 0；
       for(i = 10; i >= 0; i--)
           a = a + i;
           
       if(a >= 20)
           printf("a= [%d]\n", a);
           
        return 0;
    }




#6、链接（Links）：
有两种方式，实现链接，分别为内联方式和引用参考方式，链接的文字都是用 [方括号] 来标记。  
内联方式：  
   This is an [inline link](http://example.com/).  
   [This link](http://example.net/) has no title attribute.    
   
   I get 10 times more traffic from [Google](http://google.com/ "Google") than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or [MSN](http://search.msn.com/ "MSN Search").
   
引用参考方式：  
   I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].  

[1]: http://google.com/        "Google" 
[2]: http://search.yahoo.com/  "Yahoo Search" 
[3]: http://search.msn.com/    "MSN Search"




#7、图片（Images）：
图片的处理方式和链接的处理方式，非常的类似，允许两种样式： 行内和引用参考。   
内联方式：  
![Alt text](http://mouapp.com/Mou_128.png)   
或者  
![alt text](http://mouapp.com/Mou_128.png "Title")  

引用参考方式：  
![alt text][id] 

[id]: http://mouapp.com/Mou_128.png "Title"




#8、脚注（footnote）：  
hello[^hello]


[^hello]: hi的意思；




#9、表格：  
| Tables        | Are           | Cool  |  
| ------------- |:-------------:| -----:|  
| col 3 is      | right-aligned | $1600 |  
| col 2 is      | centered      |   $12 |  
| zebra stripes | are neat      |    $1 |  




#10、分割线：  
可以在一行中用三个或以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号中间插入空白。也在空白行下方添加三条“-”横线时。（前面讲过在文字下方添加“-”，实现的2级标题）；  
使用“*”号：  
****
* * *

使用“-”号：
- - -
- - - - - - - - -
---
--------------

使用下划线“_”：
____
_ _ _
________




#11、LaTex公式：  
行内公式：  
   这里是行内公式 $ y = x + 1 $ ;  
独立公式段：
  $$ a^2 + b^2 = c^2 $$




#12、转义字符  
支持在下面这些符号前面加上反斜杠来帮助插入普通的符号：

\   反斜杠  
`   反引号  
*   星号  
_   底线  
{}  大括号  
[]  方括号  
()  括号  
#   井字号  
+    加号  
-    减号  
.   英文句点  
!   惊叹号  



~~~mermaid
graph TD;
        A-->B;
        A-->C;
        B-->D;
        C-->E;
        D-->E;
~~~








