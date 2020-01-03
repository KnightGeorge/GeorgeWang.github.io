---
layout: post
title:  "Java Servlet实现后台"
date:  2020-01-03 21:08:26 +0530
categories: Java
---

###  Java Servlet实现网页后台 ###



1. **在eclipse中新建Dynamic web project（动态网页项目）**

2. **在Java Resources下的src下新建servlet文件**

3. **新建后，servlet的名字会被自动添加到WebContent/WEB-INF/web.xml中，当servlet被重命名后，需来此修改；web.xml中还可修改首次访问的页面或servlet**

4. **在生成的servlet文件中，会自动生成一大串由于继承（extend）来自HttpServlet的方法，一般会在其doGet方法中写网页跳转或前后端数据的交互**

   ```java
   protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   		//do something
   	}
   ```

   

5. **为避免字符集影响，一般会先设置request（请求）和response（回答）的字符集**

   ```java
   	request.setCharacterEncoding("utf-8");
   	response.setCharacterEncoding("utf-8");
   	response.setContentType("text/html;charset=utf-8");
   ```

   

6. **在前后端交互过程中，一般会用到的方法如下：**
	- **通过获取前端控件的name来获取其值**

   ```java
   	request.getParameter(“name”);
   ```
	**例如**
	
	```javascript
	<div>
		<input type="text" name="birthday">
	</div>
	```
	 **当我们想在后端获取前端的这个text文本框里的值时，我们只需要如下就可以了**
	
	```java
	String getText=request.getParameter(“birthday”)
	```
	
	- 后端向前端传数据
	
	```java
		request.setAttribute("name",obj);
	```
	
	 **当我们想往前端某个控件里传数据时，即可调用此方法进行传输，例如：**

	```java
	int age=10;
		request.setAttribute("age",age);
	```
	
	```javascript
	<div>
		<input type="text" value="${age}">
	</div>
	```
	
	- **发送request请求到某个页面或下一个动作，例如：**
	
	```java
		if(petBreed!=null && petBreed>0 ) {
			request.setAttribute("list", pBiz.queryByBreed(petBreed));
		}else {
			request.setAttribute("list", pBiz.queryByBreed(0));
		}
		request.getRequestDispatcher("list.jsp").forward(request, response);
	```
	
	**此段代码将会把此处的数据传输到list.jsp的页面中，此页面中可以通过${list}来承接此数据**
	
	- **跳转到某个网页或servlet中**
	```java
		response.sendRedirect("web");
	```
	
	**此方法通常在处理完一些事情后进行跳转到别的网页或servlet，比如注册用户成功，返回首页**