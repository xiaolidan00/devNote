# 《Java Web开发实战1200例 第1卷》读书笔记

[TOC]
##第4章jsp基础与内置对象
###4.1 jsp基本应用
定义错误页面
```jsp
//正常界面声明，若发生错误跳转到对应的异常处理界面
<%@page language="java" import"java.util" errorPage="error.jsp" pageEncoding="UTF-8" %>

//异常处理界面声明
<% page isErrorPage="true" %>

//web.xml定义统一的异常处理界面
<error-page>
<error-code>404</error-code>
<location>/error.jsp</location>
</error-page>
```

包含jsp或html文件
```jsp
<jsp:include page="headPart.jsp"></jsp:include>
```
jsp嵌套使用java程序
```jsp
<%
String[] happyStudy=["Java","HTML","SQL"];
for(int i=0;i<happyStudy.length;i++){
  %>
  <p><%=i%>------<%=happyStudy[i]%></p>
  <%
}
%>
```

获取表单提交后的相关信息
```jsp

<form name="form1" method="POST" action="?formid=1">
表单 1:<input name="text1" type="text" value="<%=text1 %>">
<input type="button" name="submit1" value="提交" onclick="checkValue()">
</form>
<p><%=message %></p>

<script>
function checkValue(){
    if(form1.text1.value==''){
        return ;
    }
    form1.submit();
}
</script>

<% 
String text1="";
String message="";
if(request.getParameter("text1")!= null ){
    text1=request.getParameter("text1");
    message="提交了第一个表单，内容为"+text1;
}
```
JSP脚本插入Javascript
```jsp
//out对象向客户端浏览器输出信息，可输出数据到html标签，可对缓冲区管理，clear()方法清楚缓冲区。clearBuffer()清除缓冲区当前内容
<%
out.println("<script>alert('Hello world!');</script>")
%>
```
指定请求转发页面
```jsp
<jsp:forward page="my_page.jsp"/>

<jsp:forward page="my_page1.jsp">
//传递参数
<jsp:param name="username" value="test"/>
<jsp:param name="password" value="123456"/>
</jsp:forward>
```

提取表单提交的信息

```jsp
<% 
   if(request.getParameter("submit")!= null ){
   request.setCharacterEncoding("UTF-8");
   String name = request.getParameter("username");
   String pwd = request.getParameter("password");
   }
   
   %>
    <p>
        <% if(name==null)||"".equals(name)){
           out.println("");
           }else{
           out.println(name);
           }
           %>
            <% if(pwd==null)||"".equals(pwd)){
           out.println("");
           }else{
           out.println(pwd);
           }
           %>
    </p>
```

request对象传递数据

```jsp
//向域范围内存放数据
<%
try{
    request.setAttribute("result","Hello World");
    User u=new User();
    u.setName("test");
    u.setPwd("123456");
     request.setAttribute("user",u);//保存Object对象
}catch(Exception e){
    request.setAttribute("result","Error");
}
%>
<jsp:forward page="deal.jsp"/>


//deal.jsp
<%
String message=request.getParameter("result");
out.println(message);
User user=(User)request.getParameter("user");
%>
```

通过cookie保存和读取信息

```jsp
<%@ page import="java.net.URLDecoder" %>
<% Cookie[] cookies=request.getCookies();

```

