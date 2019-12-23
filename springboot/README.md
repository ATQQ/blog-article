# Spring boot学习笔记

1. [Spring boot官网](https://spring.io/)
2. [官方文档](https://spring.io/guides/gs/spring-boot/)
3. [文档](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/index.html)

## Quick Start
### [手动创建](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/getting-started-first-application.html)
1. 创建maven 工程
2. 配置pom.xml文件 新增依赖
```xml
<parent>
    <groupId>org.springframework.boot<groupId>
    <artifactId>spring-boot-starter-parent<artifactId>
    <version>2.1.6.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot<groupId>
        <artifactId>spring-boot-starter-web<artifactId>
    </dependency>
</dependencies>
```
3. 创建Example.java
   * 目录:src/main/java/Example.java
```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

	@RequestMapping("/")
	String home() {
		return "Hello World!";
	}

	public static void main(String[] args) {
		SpringApplication.run(Example.class, args);
	}

}
```

### [自动创建](https://start.spring.io/)
1. 填完信息自动生成项目文件 点击下载即可
2. 导入到IDE里面
3. 运行DemoApplication
4. localhost:8080访问查看运行结果

## 注解开发
### 1. @SpringBootApplication
   
等价于@SpringBootConfiguration+
@EnableAutoConfiguration+
@ComponentScan

```java
//@SpringBootApplication (一个替代下面三个)
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```

### 2. @RestController
等价于 @Controller+@ResponseBody

注:@RestController与@RequestMapping是Spring MVC的注解,不是Spring Boot特有的


### 3.@RequestMapping,@PathVariable
```java
    /**
     * 测试Restful协议 从路径中获取字段
     * @param cityId cityId
     * @param userId userId
     * @return
     */
    @RequestMapping(value = "/{city_id}/{user_id}",method = RequestMethod.GET)
    public Object findUser(@PathVariable("city_id") String cityId, @PathVariable("user_id") String userId){
        params.clear();
        params.put("cityId",cityId);
        params.put("userId",userId);
        return params;
    }
```


### 4.@GetMapping
简化method设置
```java
 /**
     * 测试GetMapping
     * @param from
     * @param size
     * @return
     */
    @GetMapping("/v1/page_user1")
    public Object getUser(int from,int size){
        params.clear();
        params.put("from",from);
        params.put("size",size);
        return params;
    }
```

### 5.@RequestParam

```java
    /**
     * 测试RequestParam 设置默认值
     * @param from
     * @param size
     * @return
     */
    @GetMapping("/v1/page_user2")
    public Object getUser2(@RequestParam(defaultValue = "100",name = "page") int from,int size){
        params.clear();
        params.put("from",from);
        params.put("size",size);
        return params;
    }
```

### 6.@RequestBody
请求头的 Content-Type需设置为application/json,参数放在body中
```java
    /**
     * 测试RequestBody
     * @param user
     * @return
     */
    @PostMapping("/v1/page_user3")
    public Object getUser3(@RequestBody User user){
        params.clear();
        params.put("user",user);
        return params;
    }
```

### 7.@RequestHeader

```java
    /**
     * 测试RequestHeaders
     * @param token
     * @param id
     * @return
     */
    @GetMapping("/v1/page_user4")
    public Object getUser4(@RequestHeader("access_token")String token,String id){
        params.clear();
        params.put("access_token",token);
        params.put("id",id);
        return params;
    }
```

### 8.HttpServletRequest

```java
/**
     * HttpServletRequest
     * @param request
     * @return
     */
    @GetMapping("/v1/page_user5")
    public Object getUser5(HttpServletRequest request){
        params.clear();
        params.put("id",request.getParameter("id"));
        return params;
    }
```

## 其它Http请求

```java
    private Map<String,Object> params=new HashMap<>();

    @PostMapping("/test/post")
    public Object testPost(Integer id){
        params.clear();
        params.put("id",id);
        return params;
    }

    @DeleteMapping("/test/del")
    public Object testDel(Integer id){
        params.clear();
        params.put("id",id);
        return params;
    }

    @PutMapping("/test/put")
    public Object testPut(Integer id){
        params.clear();
        params.put("id",id);
        return params;
    }
```

## json处理(jackson)
```java
public class Student {
    // 使用别名
    @JsonProperty("userName")
    private String name;
    // 省略
    @JsonIgnore
    private Integer age;
    // 空字段不返回
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private Character sex;
    // 日期格式化
    @JsonFormat(pattern = "yyyy-MM-dd hh:mm:ss",locale = "zh",timezone = "GMT+8")
    private Date birthday;
    // 省略 getter and setter
    //constructor
}

```

## spring-boot 目录结构
```text
src/main/java:存放代码
sec/main/resource:
    static:存放静态文件 
    templates:存放静态页面文件
    config:存放配置文件
    resource:存放其它资源
```

**同名静态资源**,加载顺序

META/resources->resources->static->public 有则直接返回,没有则404


## 引入thymeleaf
```xml
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-thymeleaf<artifactId>
	</dependency>
```
默认的模板映射路径是：src/main/resources/templates，

```java
@Controller
public class PageController {

    @RequestMapping(value = "/page/index")
    public String index(){
        return "index";
    }
}
```
访问路径:localhost:8080/page/index

## 其它静态资源
```text
src/main/resource
    static
        images
        css
        js
        others
```

## 默认配置
* spring.resource.static-locations=
  * classpath:/META-INF/resources/
  * classpath:/resources/
  * classpath:/static/
  * classpath:/public/

## 更改配置写入自定义静态资源文件夹
resources目录下创建application.properties,加入test目录
```properties
spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/,classpath:/test/
```


## 文件上传

服务端
```java
private static final String SaveDirector="D:\\documents\\study\\maven\\project\\demo\\src\\main\\resources\\static\\images";
    @PostMapping("/file/upload")
    public Object uploadFile(MultipartFile file, HttpServletRequest request){
        if(file.isEmpty()){
            return null;
        }

        String filename = file.getOriginalFilename();
        System.out.println(filename);

        String suffix=filename.substring(filename.lastIndexOf("."));

        filename= UUID.randomUUID()+filename.substring(0,filename.lastIndexOf("."));

        filename=filename+suffix;
        System.out.println(filename);

        File saveFile=new File(SaveDirector+"\\"+filename);

        try {
            file.transferTo(saveFile);
        } catch (IOException e) {
            e.printStackTrace();
            return new ResJSON(20001,filename,"上传失败");
        }
        return new ResJSON(200,filename,"上传成功");
    }
```

html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>
<input type="file" id="file">
<button type="button" onclick="sureUpload()">上传</button>
<button type="button">取消</button>
<script>
    let xhr=null;
    //开始上传
    function sureUpload() {
        const file=document.getElementById('file').files[0],
        form=new FormData();
        xhr=new XMLHttpRequest();
        form.append("file",file);
        xhr.open("post","/file/upload",true);
        xhr.onload=uploadSuccess;
        xhr.onerror=uploadError;

        xhr.onprogress=process;

        xhr.send(form);
    }

    function uploadSuccess(e) {
        console.log(JSON.parse(e.currentTarget.response));
        alert("上传成功");
    }

    function uploadError(err) {
        console.log(err);
        alert("上传失败");
    }

    function cnacelUpload() {
        xhr.abort();
    }

    function process(e) {
        console.log("-----");
        console.log(e.lengthComputable);
        let max=e.max;
        let loaded=e.loaded;
        console.log(max,"---",loaded)
    }
</script>
</body>
</html>
```

## 硬编码方式修改文件上传大小限制
```properties
# 上传文件大小限制
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```

springProApplication文件(带有@SpringBootApplication注解)中添加
```java
	@Bean
	public MultipartConfigElement multipartConfigElement(){
		MultipartConfigFactory factory=new MultipartConfigFactory();
		factory.setMaxFileSize("10240KB");

		factory.setMaxRequestSize("102400KB");

		return factory.createMultipartConfig();
	}
```


## 打包jar包

1. 引入maven插件
```xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```
2. 打包命令
```maven
mvn package
```

3. 运行命令(Running as a Packaged Application)
```text
java -jar target/myapplication-0.0.1-SNAPSHOT.jar
```
## 指定文件上传预览路径

配置application.properties
```properties
spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/,classpath:/test/,file:D:/documents/study/pic/
```

## 文件服务器 
* fastdfs
* 阿里云oss
* nginx搭建文件服务器

## [spring-boot热部署devtools](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/html/using-boot-devtools.html)
pom引入插件
```xml
<!--热更新插件-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
</dependency>
```

**注意**:IDEA中需进行以下[设置](https://blog.csdn.net/YuenBin128/article/details/80179472)

### 不被热部署的文件
默认:/META-INF/maven,/META-INF/resources,/resources,/static,/public,/template

### 指定不进行热部署的文件
* 指定不监听application.properties文件,
在配置application.properties,中添加
```properties
spring.devtools.restart.exclude=application.properties
```
* 通过触发器,控制什么时候进行热更新
```properties
spring.devtools.restart.trigger-file=.reloadtrigger拦截器

```

## spring配置文件
[默认文档](https://docs.spring.io/spring-boot/docs/2.1.8.RELEASE/reference/htmlsingle/#common-application-properties)

## 配置文件自动映射到属性和实体类方式
1. 配置文件加载
   * 方式1
     * Controller上配置 @PropertySource({"classpath:xxxxx.properties"})
     * xxxxx.properties
     ```properties
     web.file.uploadDirectory=D:/documents/study/pic
     ```
     * 取值
     ```java
        @Value("${web.file.uploadDirectory}")
         private  String saveDirectory="";
     ```
    * 方式2
      * 使用实体类配置文件
        * @Component 注解
        * @PropertySource 注解指定配置文件位置
        * WConfigurationProperties 注解设置相关属性
      * 使用
      
      必须用过注入IOC对象Resource,才能在类中获取到配置文件的值
      ```java
      @Autowired
      private ServerSettings serverSettings;
      ```
     * 示例
       * ServerSettings.java
        ```java
        @Component
        @PropertySource({"classpath:common.properties"})
        //如果带有prefix属性则可忽略@Value注解,自动根据配置文件同名自动注入
        @ConfigurationProperties(prefix = "test")
        public class ServerSettings {
            //@Value("${test.name}")
            private String name;
            //@Value("${test.domain}")
            private String domain;
            //省略get-set方法
        }  
        ```
        * common.properties
        ```properties
        #测试实体类配置文件
        test.domain=sugarat.top
        test.name=spring-boot
        ```
        * testController.java
        ```java
        @Autowired
        private ServerSettings serverSettings;

        @GetMapping("/test/properties")
        public Object getProperties(){
            return serverSettings;
        }
        ```

# 单元测试
## 引入依赖
```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
```
### 普通测试
```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {DemoApplication.class})
public class FirstTest {

    @Test
    public void firsttest(){
        System.out.println("success");
        //断言    
        TestCase.assertEquals(2,3);
    }
    // 测试之心之前启动
    @Before
    public void testBefore(){
        System.out.println("before");
    }
    // 执行之后启动
    @After
    public void testAfter(){
        System.out.println("after");
    }
}
```

## API测试
```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {DemoApplication.class})
@AutoConfigureMockMvc
public class MockMvcTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testGetApi() throws Exception {
        // perform 执行一个RequestBuilder请求
        // andExpect添加ResultMatcher验证规则
        // andReturn 最后返回对应的MvcResult
        MvcResult result = mockMvc.perform(MockMvcRequestBuilders.get("/mock/test1"))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andReturn();
        int status = result.getResponse().getStatus();
        
        System.out.println("success"+status);
    }
}
```

## 个性化启动banner设置和debug日志
* banner.txt
```txt
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: 自定义文字 spring boot::        (v2.1.8.RELEASE)
```
* application.properties
```properties
spring.banner.location=banner.txt
```
* 打成jar包使用命令运行即可查看 启动debug日志
```cmd
java -jar target/myapplication-0.0.1-SNAPSHOT.jar --debug
```

## 配置全局异常处理
```java
@RestControllerAdvice
public class GlobalExHandler {

    /**
     * 全局异常
     * @param e
     * @param request
     * @return
     */
    @ExceptionHandler(value = Exception.class)
    Object handleGlobalException(Exception e, HttpServletRequest request){
        Map<String,Object> res=new HashMap<>();
        res.put("code",555);
        res.put("msg",e.getMessage());
        res.put("url",request.getRequestURI());
        return res;
    }
}

```

## 自定义异常捕获
* MyException.java
```java
public class MyExcepion extends RuntimeException {

    private Integer code;

    private String msg;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public MyExcepion(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}

```

* HandleException.java
```java
@RestControllerAdvice
public class GlobalExHandler {
    /**
    * 捕获自定义异常
    */
    @ExceptionHandler(value = MyExcepion.class)
    Object handleMyException(MyExcepion e, HttpServletRequest request){
        Map<String,Object> res=new HashMap<>();
        res.put("code",e.getCode());
        res.put("msg",e.getMsg());
        res.put("url",request.getRequestURI());
        return res;
    }
}

```

## 部署war包于tomcat中
* maven常规打包成war包放入tomcat Webapps目录

## 配置自定义过滤器
* 启动类中添加
```java
@ServletComponentScan
```
* 创建一个Filter
```java
/**
 * urlPatterns支持正则
 */
@WebFilter(urlPatterns ={"/api/user/*"},filterName = "userFilter")
public class UserFilter implements Filter {


    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("user Filter Init");
    }

    @Override
    public void destroy() {
        System.out.println("user Filter destroy");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest req= (HttpServletRequest)servletRequest;
        HttpServletResponse resp=(HttpServletResponse) servletResponse;

        String username=req.getParameter("username");
        if(username.equals("admin")){
            filterChain.doFilter(servletRequest,servletResponse);
        }else{
            return;
        }
    }
}

```

## Servlet3.0注解自定义原生Servlet
```java
@WebServlet(name = "userServlet",urlPatterns = "/user/servlet/*")
public class UserServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(req.getRequestURI());
        resp.setContentType("application/json;charset=utf-8");
        resp.getWriter().print("{name:\"小明\"}");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }
}

```

## Servlet3.0自定义原生Listerner
```java

@WebListener
public class RequestListener implements ServletRequestListener {

    private static Integer count=0;

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        
    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        count++;
        System.out.println(count);
    }
}


@WebListener
public class ContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("init Context");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("destryy Context");
    }
}

```

## spring boot2.0拦截器
* CustomWebMvcConfigurer.java
```java
@Configuration
public class CustomWebMvcConfigurer  implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 注册拦截器
        registry.addInterceptor(new UserIntercepter()).addPathPatterns("/api/user/**");
        WebMvcConfigurer.super.addInterceptors(registry);
    }
}
```
* UserIntercepter.java
```java
public class UserIntercepter implements HandlerInterceptor {

    /**
     * 进入controller之前
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println(request.getRequestURI());
        System.out.println("UserIntercepter--->preHandle");

        return request.getParameter("username").equals("admin");
    }

    /**
     * 进入controller之后,是图渲染之前
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("UserIntercepter--->postHandle");
    }

    /**
     * 执行controller之后
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("UserIntercepter--->afterHandle");
    }
}

```
* 补充
  * /* 代表目录
  * /**表示所有

## 整合Mybatis
### 引入依赖
```xml
 <dependency>
     <groupId>org.mybatis.spring.boot</groupId>
     <artifactId>mybatis-spring-boot-starter</artifactId>
     <version>1.3.2</version>
 </dependency>

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>

<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```
### 修改启动类 增加 mapperScan扫描mapper
```java
@SpringBootApplication
@MapperScan(value = "com.sugar.demo.mapper")
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

	@Bean
	@ConfigurationProperties(prefix = "spring.datasource")
	public DataSource druidDataSource(){
		DruidDataSource druidDataSource=new DruidDataSource();
		return druidDataSource;
	}
}
```

### 配置数据源
```properties
#UTC 国际标注时
#GMT%2B8 北京东八区
spring.datasource.url=jdbc:mysql://localhost:3306/springboot?serverTimezone=GMT%2B8
spring.datasource.username=sugar
spring.datasource.password=a123456
#切换数据源 
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
#打印sql
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

### 编写mapper
* 使用#替代$ 防止sql注入
```java
public interface UserMapper {

    @Insert("insert into user(username,sex,phone,createAt) values(#{username},#{sex},#{tel},#{createData})")
    @Options(useGeneratedKeys = true,keyProperty = "id",keyColumn = "id")
    int insert(User user);
}
```

### service调用
```java
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public int addUser(User user) {
        return userMapper.insert(user);
    }
}
```

### controller调用
```java
@RestController
@RequestMapping("user")
public class UserController {

    @Autowired
    private UserService userService;

    private HashMap<String,Object> res=new HashMap<>();

    @PostMapping("add")
    public Object registerUser(){
        User test=new User(null,"sugar",'F',"15196520474",new Date());
        res.clear();
        res.put("code",200);
        res.put("data",userService.addUser(test));
        return res;
    }
}
```
### 其它mapper

```java
public interface UserMapper {

    /**
     * 新增用户
     * @param user 新用户数据
     * @return  新增用户id
     */
    @Insert("insert into user(username,sex,phone,create_date) values(#{username},#{sex},#{tel},#{createData})")
    @Options(useGeneratedKeys = true,keyProperty = "id",keyColumn = "id")
    int insert(User user);


    /**
     *  查询所有的用户信息
     * @return 用户列表
     */
    @Select("select * from user")
    @Results({
            @Result(column = "create_date",property = "createData"),
            @Result(column = "phone",property = "tel"),
            @Result(column = "id",property = "id")
    })
    List<User> getAll();


    /**
     * 通过id删除User
     * @param userId 用户id
     */
    @Delete("delete from user where id = #{userId}")
    void delete(Integer userId);

    /**
     * 通过id更新用户名字段
     * @param user 新数据
     */
    @Update("update user set username=#{username} where id =#{id}")
    void update(User user);
}
```

# 事务

## 事务出错回滚
```java
@PostMapping("testEvent")
    @Transactional(propagation = Propagation.REQUIRED)
    public Object testEvent(){
        User test=new User("admin",12,"M","15196520474",new Date());
        res.clear();
        res.put("code",200);
        res.put("data",userService.addUser(test));
        int i=1/0;
        return res;
    }
```

# Redis

## 相关链接
>[在线命令测试](https://try.redis.io/)<br>
>[redis官网](https://redis.io/)<br>
>[菜鸟教程](https://www.runoob.com/redis/redis-tutorial.html)
### linux下
```
下载
wget http://download.redis.io/releases/redis-5.0.5.tar.gz

解压
tar xzf redis-5.0.5.tar.gz

编译源码
cd redis-5.0.5
make

启动服务
./src/redis-server

连接服务
./src/redis-cli

测试
set test hello-world
get test
```

# 整合redis
## 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## 简单使用
```java
public class testRedisController {

    private HashMap<String,Object> res=new HashMap<>();


    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @PostMapping("set")
    public Object set(String key,String value){
        res.clear();
        res.put("key",key);
        res.put("value",value);
        stringRedisTemplate.opsForValue().set(key,value);
        return res;
    }

    @GetMapping("get")
    public Object get(String key){
        res.clear();
        res.put("key",key);
        res.put("value",stringRedisTemplate.opsForValue().get(key));
        return res;
    }
}

```

## Redis桌面应用
* 链接:[https://redisdesktop.com/](https://redisdesktop.com/)

## 封装工具类
* redis操作工具类
* json与 Object相互转换
* jsonData回调模板

# 定时任务与异步任务处理
## 定时任务
### 启动类增加新注解
```java
@EnableScheduling
```

### 使用 (testTimerTask.class)
```java
@Component
public class TestTimerTask {
    //每隔5s执行一次
    @Scheduled(fixedRate = 1000*5)
    // 每隔2s执行一次
    // @Scheduled(cron = "*/2 * * * * *")
    // @Scheduled(fixedDelay=3000)
    public void test1(){
        System.out.println(new Date());
    }
}
```
* @fixedRate 多久执行一次
* @fixedDelay 在上一次执行结束之后再隔 多久执行一次
* @fixedDelayString 采用字符串作为参数

## 异步任务
### 启动类添加注解
```java
@EnableAsync //开启异步任务
```
### 异步任务类

```java
@Component
public class AsyncTask {

    @Async
    public void task1() throws InterruptedException {
        long begin = System.currentTimeMillis();
        Thread.sleep(3000L);
        System.out.println(System
                .currentTimeMillis() - begin);
    }
}

```
### 业务调用

```java
@RequestMapping("asnyc")
@RestController
public class testAsyncController {

    @Autowired
    private AsyncTask AsyncTask;

    @GetMapping("task1")
    public String testAsyncTask() throws InterruptedException {
        AsyncTask.task1();
        return "success";
    }
}
```

### 等待异步回调完成
```java
    // 异步函数
    @Async
    public Future<String> task2() throws InterruptedException {
        long begin = System.currentTimeMillis();
        Thread.sleep(5000L);
        System.out.println(System
                .currentTimeMillis() - begin);
        return new AsyncResult<String>("任务2完成结束");
    }

    // 等待回调完成
    Future<String> task2 = AsyncTask.task2();
    while (true){
        if(task2.isDone()){
            break;
        }
    }
```