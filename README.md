
#cTemplate模板引擎

---

###0.0.9：
	cTemplate 自己写的一个JS模板引擎，性能稳定，简单好用。自测无BUG。过两天补充说明文档;过一段时间会发布最终稳定1.0.0版本




### 使用说明

#### 0、调用引擎方式



##### （1）、数据结构是一个JSON，如：

```
var data={
    title:'CTemplate',
    list:[
      'test data 1',
      'test data 2',
      'test data3'
    ]
}
```



##### （2）、CTemplate占用C.template命名空间

```
//可以付值给一个短名变量使用
var bt = C.template;
```



##### （3）、方法一：tpl是传入的模板(String类型)，可以是模板的id，可以是一个模板片段字符串，传入模板和对应数据返回对应的HTML片段

```
//方法一：直接传入data，返回编译后的HTML片段
var html0 = C.template(tpl,data);  

//或直接传入id即可
var html0 = C.template('test1',data);  
```



##### 方法二：或者可以只传入tpl，这时返回的是一个编译后的函数，可以把这个函数缓存下来，传入不同的data就可以生成不同的HTML片段

```
//方法二：先不传入data，返回编译后的函数
var fun = C.template(tpl);
//或直接传入id即可
var fun = C.template('test1');

//之后通过改变数据，调用缓存下来的函数，产生不同的HTML片段
var html1 = fun(data1);
var html2 = fun(data2);
```



##### （4）、最后将他们插入到一个容器中即可

```
document.getElementById('result1').innerHTML=html1;
document.getElementById('result2').innerHTML=html2;
document.getElementById('result3').innerHTML=html3;
```



#### 模板基本语法（默认分隔符为<% %>，也可以自定义）



##### 分隔符内语句为js语法，如：

```
<% var test = function(){
    //函数体
};
if(1){}else{};
function testFun(){
    return;
};
%>
```



##### 假定事先设置数据为

```
var data={
    title:'CTemplate',
    list:['test1<script>','test2','test3']
}
```



##### 注释

```
<!-- 模板中可以用HTML注释 -->  或  <%* 这是模板自带注释格式 *%>

<% //分隔符内支持JS注释  %>
```



##### 输出一个变量

```
//默认HTML转义，如果变量未定义输出为空
<%=title%>  

//不转义
<%:=title%> 或 <%-title%>

//URL转义，UI变量使用在模板链接地址URL的参数中时需要转义
<%:u=title%>

//UI变量使用在HTML页面标签onclick等事件函数参数中需要转义
//“<”转成“&lt;”、“>”转成“&gt;”、“&”转成“&amp;”、“'”转成“\&#39;”
//“"”转成“\&quot;”、“\”转成“\\”、“/”转成“\/”、\n转成“\n”、\r转成“\r”
<%:v=title%>

//HTML转义（默认自动）
<%=title%> 或 <%:h=title%>
```



##### 判断语句

```
<%if(list.length){%>
    <h2><%=list.length%></h2>
<%}else{%>
    <h2>list长度为0<h2>
<%}%>
```



##### 循环语句

```
<%for(var i=0;i<list.length;i++){%>
        <li><%=list[i]%></li>
<%}%>
```



#### 4、不推荐使用功能



##### 用户可以自定义分隔符，默认为 <% %>，如：

```
//设置左分隔符为 <!
C.template.LEFT_DELIMITER='<!';

//设置右分隔符为 <!  
C.template.RIGHT_DELIMITER='!>';
```



##### 自定义默认输出变量是否自动HTML转义

```
//设置默认输出变量是否自动HTML转义，true自动转义，false不转义
C.template.ESCAPE = false;
```





