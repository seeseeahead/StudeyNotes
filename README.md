# Test
Vue
@Click:按钮点击弹窗；
v-model:输入框
v-show:true显示，false不显示
v-for:
v-for=" stu in students" :key="stu.id">{{ stu.id }}{{ stu.name }}
v-
router路由

Spring注解
@RestController：//请求处理类
@RequestMapping：//处理的请求调用哪个方法
@GetMapping

@Component： //将当前类交给容器管理，成为IOC容器中的bean 
@Autowired： //运行时，IOC容器会提供bean变量，并赋值给该变量 ~ 依赖注入，为spring里的方法
@Primary：//多个bean变量时，使用该注解来使他注解下的类被调用
@Qualifier("empServiceA")： //多个bean变量时，使用该注解来使（）中引用的类被用
@Resource(name = "empServiceB")：//为jdk里的方法，多个bean变量时，不使用@Autowired；直接用@Resource该注解来使（）中引用的类被用，
@WebFilter(urlPatterns = "拦截路径") ：Filter类
@ServletComponentScan ：/开启了对servlet组件的支持：Filter启动类
@Configuration ：说明当前类为配置
@RestControllerAdvice //=@RequestBody + @ControllerAdvice
@ExceptionHandler(Exception.class)//捕获所有异常
@Transactional  //注解写方法前表示为spring事物管理


Spring快捷键
Ctrl + alt + t：try/if等等

插件
Grep console

文件上传

 


会话 jwt令牌、过滤器、拦截器、异常处理器
浏览器向服务器端发送请求，服务器接收请求并响应，一次会话可以有多次请求

	方式：
Cookie：支持https协议
Session：不能跨域
令牌：哪都好
	Jwp令牌：Json数据格式
组成：Header（头）【记录令牌类型，签名算法】+ payload（有效载荷）【携带有效信息、默认信息】+ signature（签名）【将header，payload并加入密钥，指定算法计算而来】
	Base64：是一种基于64个可打印字符（A-Z a-z 0-9 + /）来表示二进制数据的编码方式
场景：
登陆成功后，生成令牌；后续请求携带令牌先校验令牌有没有再校验有没有效，再处理请请求。
 	生成令牌：Jwts.builder()
Jwts.builder()
        .signWith(SignatureAlgorithm.HS256,"itheima")//签名算法
        .setClaims(claims)//自定义内容
        .setExpiration(new Date(System.currentTimeMillis() + 3600*1000))//设置有效期
        .compact();

 	解析令牌：Jwts.parser()
wts.parser()
        .setSigningKey("itheima")//给定签名密钥
        .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9tIiwiaWQiOjEsImV4cCI6MTY5MDg4ODkxN30.aLfOa5QJvHHPt9NyGoxeQwkqEF3PVq6qqsn_AfNE1ng")
        .getBody();//拿到jwt的payload（自定义）部分



	过滤器Filter ：拦截所有资源
把资源请求拦截下来，经过一系列特殊处理，通过之后才可放行请求去访问资源，然后响应的资源再回到过滤器，最后响应给浏览器
	方法
Init：初始化方法，只会被调用一次
doFilter：拦截到请求之后使用，会被调用多次
destroy：销毁方法，只会调用一次
	注解：
Filter类@WebFilter(urlPatterns = "拦截路径") 
启动类@ServletComponentScan //开启了对servlet组件的支持
	拦截路径：
具体路径：/路径值，例如/login，访问login时会被拦截
目录拦截：/目录/*，访问该目录下的所有资源会被拦截
拦截所有：/*
Map：默认是由数组和链表组成

	拦截器 interceptor ：拦截进入spring的资源
1.定义拦截器，实现HandlerInterceptor接口，并重写preHandle
，postHandle。afterCompletion三个方法；
2.注册拦截器
@Configuration //说明当前类为配置
需要拦截资源：addPathPatterns
不需要要拦截资源：excludePathPatterns
拦截路径：
/*：一级路径；
/**：任意就级路径；
/depts/*：depts下的一级路径
/depts/**：depts下的任意级路径

	全局异常处理器
创建一个异常处理器
注解：
类：@RestControllerAdvice 
	@RestControllerAdvice =@RequestBody+@ControllerAdvice
方法：@ExceptionHandler(Exception.class)//捕获所有异常


Spring事物回滚
@Transactional  //注解写方法前表示为spring事物管理，出现异常会回滚，Transactional只会回滚运行时（RuntimeException
）处理异常

rollbackFor = 异常类型：表示需要回滚的异常类型，Exception.class为所有异常
@Transactional(rollbackFor = Exception.class)//回滚所有异常

	事物属性-传播行为（propagation = 属性值）：指一个事物方法被另一个事物方法调用时，这个事物方法应该如何进行事物控制。
	属性值：
REQUIRES：需要事物，有就加入，没有就新建
REQUIRES_NEW：已有事务，新建事务，防止事务之间影响


Aop 面向切面、方法编程
 	配置：
引入依赖：spring-boot-starter-aop
类注解：@Aspect //Aop类
方法注解（作用范围）：@Around("execution(* com.itheima.service.*.*(..))")//切入点表达式
场景：记录操作日志、权限控制、事务管理等
 	优势：代码无侵入、减少重复代码、提高开发效率、维护方便

	通知：
	类型
@Around：环绕通知，此方法目标方法执行前后都执行
环绕通知需要调用proceedingJoinPoint.proceed()方法来让原方法执行，返回值必须指定为Object类来接收是否完成的信息
@Before：前置通知，此方法目标方法执行前执行
@After：后置通知，此方法目标方法执行后执行，无论异常
@AfterReturning：返回后通知，此方法在标注的通知方法执行后执行，有异常不执行
@AfterThrowing：异常后通知，此方法在标注的通知方法执行出现异常后执行
 	引用切入点表达式：
@Pointcut（”切入点表达式”）注解一个无参方法来被调用来指定作用范围
 	切入点表达式
1.直接按格式写
execution(方法类型 包名 类名 方法名（返回值类型）)
execution(* com.itheima.service.DeptService.*(..))
	通配符
*：匹配单个任意字符
..:匹配多个任意字符
2.注解形式
(1)	创建一个注解类并标注
@Retention(RetentionPolicy.RUNTIME) //注解何时生效
@Target(ElementType.METHOD) //说明标识的是方法
(2)	在切面类中用切面注解@Pointcut引入注解类
@Pointcut("@annotation(com.itheima.aop.注解类名)")
(3)	在需要的方法上加上注解@注解类名 引用即可

	通知顺序（栈，先进后出）
1.按字母顺序，字母靠前前置通知先执行，后置通知后执行
2.类注解@Order(数字)：数字小的类里的前置通知先执行，后置通知后执行








其他命令
关闭nginx命令：nginx -s quit




单词
generate  #产生
claims   #要求
match #匹配
duplicate #重复
propagation #传播
Participating in existing transaction #参与一个已经存在的事物
Initiating transaction rollback #正在启动事务回滚
Suspending current transaction #挂起当前事务
record #记录
Signature #签名
