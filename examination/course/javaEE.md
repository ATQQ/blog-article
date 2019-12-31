# 1. java EE 复习

## 列举三种动态网页技术，并描述其特点
* ASP
  * Active Server Pages 基于.Net实现的动态服务器页面
* JSP
  * jAVA Server Page,基于Java实现的web页面模板引擎
* Django
  * 基于Python语言实现的MVC设计模式的WEB框架
* PHP 
  * 主要应用于Web开发,服务端执行的脚本语言

## 描述你在使用某集成开发环境，进行JSP+JavaBean+Servelet编程时，遇到的一个故障，及最终的解决办法
使用IDEA 集成开发环境,在新建Servlet时会自动生成@WebServlet注解,避免了配置Web.xml的麻烦,但是启动项目后发现并没有起作用,原因是 @WebServlet("test")没有加 <code>/</code> 正确写法为WebServlet("/test)

## 编程题
---
>Servlet接收表单传递过来的数据
假定给你提供了一个HTML文件，用户在表单中输入某些信息(可能含有中文)，用户点提交按钮的时候，向Servlet提交。
你的Servlet的功能：对数据的合法性进行基本检验，如果有问题则给出错误消息。如果满足要求，则直接输出html到客户端，回显用户输入的每一项信息。要求：中文不能有乱码的情况。

```java
@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //设置回调字符编码和文本格式
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html");
        PrintWriter writer=response.getWriter();

        //获取表单传递的参数
        String username=request.getParameter("username");
        String password=request.getParameter("password");

        //数据校验
        if(username==null||username.equals("")){
            writer.println("用户名不能为空");
            return;
        }
        if (password.length()<6||password.length()>12){
            writer.println("密码长度不符合要求");
            return;
        }
        
        //成功回调
        writer.println("账号:"+username);
        writer.println("密码:"+password);

    }
```

---
>JSP输出基本信息创建一个Welcome.jsp，在页面上显示：
当前日期时间是2019-12-15 19:20:30，
欢迎你，来自210.41.240.254的朋友。
其中的日期时间和IP都是动态获取的，当前页面显示3秒后，
跳转到：https://www.swpu.edu.cn/scs/


```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>欢迎</title>
</head>
<body>
    <%
        DateFormat df=new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        Date now =new Date();
        String nowTime=df.format(now);
        String ip=request.getRemoteAddr();
    %>

    当前日期时间是<%=nowTime %> ，欢迎你，来自 <%=ip%> 的朋友。

    <%
        response.setHeader("refresh","3;url=https://www.swpu.edu.cn/scs/");
    %>
</body>
</html>
```

---
>会话处理能力
用JSP或Servlet实现登录的表单界面。当用户成功登录后，将用户名存储在Cookie中，下一次打开此页面时，用户名可以自动显示在用户名文本框内。

* servlet
```java
    //获取表单传递的参数
    String username=request.getParameter("username");
    String password=request.getParameter("password");
    PrintWriter writer=response.getWriter();
    //成功回调
    writer.println("账号:"+username);
    writer.println("密码:"+password);
    //存储cookie
    response.addCookie(new Cookie("username",username));
    response.addCookie(new Cookie("password",password));
```

* jsp
```jsp
<%
        <!-- 数据获取 -->
    Cookie[]cookies =request.getCookies();
    String username="";
    String password="";
       for (Cookie k:cookies){
           if(k.getName().equals("username")){
               username=k.getValue();
           }
           if(k.getName().equals("password"){
               password=k.getValue();
           }
       }
%>

<form name="regForm" action="/hello_servlet/LoginServlet" method="post">
        
            用户名：
            <input type="text" name="username" value="<%=username%>" />
            密码：
            <input type="password" name="password" value="<%=password%>"/>
            <input type="submit" value="提交">
</form>
```

---
>Java类封装数据库访问
编写一个名为DbHelper的类，此类能连接到SQL Server数据库中指定的数据库，并提供一个checkUser的方法，接收用户名和密码，到tblUser表中，检查此用户是否存在。方法返回int类型，返回1表示用户名和密码正确，返回-1表示不能成功登录。

```java
public class DbHelper {

    private Connection connection=null;

    /**
     构造方法，尝试创建连接对象
     */
    public DbHelper() throws Exception
    {
        String sJdbcURL="jdbc:sqlserver://localhost:1433;databaseName=sugar";
        String sUser="sa";
        String sPwd="123456";
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        connection= DriverManager.getConnection(sJdbcURL,sUser,sPwd);
    }

    public int checkUser(String username,String password)
    {
        try
        {
            CallableStatement call=connection.prepareCall("{call prjCheckUser(?,?)}");
            call.setString(1,username);
            call.setString(2,password);
            ResultSet rs=call.executeQuery();
            rs.next();
            return rs.getString(1).equals("Y")?1:-1;
        }
        catch(Exception err)
        {
            return -1;
        }
    }

}
```