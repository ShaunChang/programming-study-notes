
# 软件开发现有问题
开始：开发完毕，初次使用

结束：软件升级和维护，代价非常大，大到不如重新开发。但是重新开发意味着以前的努力白费了。所以出现了一些设计思想和框架 为的是实现软件生命周期的利益最大化。你可以不修改，去添加新的类（开闭原则），实现代码的升级。但是原来的代码中是new的原来的对象，意味着你还得改代码 即存在耦合

 
# 0 什么是微服务
-整个应用必须构建成一系列小服务的的组合  
eg：把原来mvc中的esrvice单独弄成一个模块 这个模块只提供接口  
-原来的单体架构：把所有服务都封装在一个应用中                                     
好处：易于开发和测试 吧war包复制多份放到服务器就好了  
坏处：难于维护 动一个 其它都要重新部署
-微服务：每个功能独立出来 用的时候用哪个调用哪个 动态组合
好处：节省资源 每个独立 好维护  


# 1 sp什么东西
-不用写配置文件的ss框架（springboot用注解）  
-spring 框架有两个主要原则：依赖注入 控制反转  
-特性  
使用 Spring 项目引导⻚面可以在几秒构建一个项目  
Spring容器、日志、自动配置AutoCongfiguration、Starters，方便对外输出各种形式的服务，如 REST API、WebSocket、Web、Streaming、Tasks  
非常简洁的安全策略集成  
支持关系数据库和非关系数据库  
支持运行期内嵌容器，如 Tomcat、Jetty  
强大的开发包，支持热启动  
自动管理依赖  
自带应用监控  
支持各种 IDE，如 IntelliJ IDEA 、NetBeans  
-与tmocat的关系  
它把tomcat直接内嵌了  以前弄完app要把代码部署到tomcat中（其实就是把各种文件放到固定的文件夹下） 现在就是sp直接给你把这些弄好了
-约定大于配置
就是默认给你弄了一堆配置。你按约定来，比如约定port要去ymal文件这样改springboot：port：5000。那你这样改完就会生效。比如约定你什么都不弄。默认一些配置就生效。
你别乱动，这些东西默认就有。
好处：在没有这个的时候，所有的东西，所有。我们需要去手动一个一个配。
坏处：有的约定默认配置人家弄了你不知道

# IOC AOP
### 软件设计原则SOLID
    SOLID（单一功能、开闭原则、里氏替换、接口隔离以及依赖反转）

### IOC
    控制反转。是对SOLID设计原则中的依赖倒置原则（高层模块不应该依赖于低层模块，它们都应该依赖于抽象；抽象不应该依赖于具体实现，具体实现应该依赖于抽象）的实现。
    反转的是对资源对控制，以前你是地主（你的代码），自己份内对土地资源都归你自己管 你想怎么弄就怎么弄。现在土地被收了，统一归国家（spring）管。你只管负责种地（代码逻辑），土地人家给你那块你就用那块，你变成了要着吃的货色。
    
    这样做有什么好处呢？回到代码解释：
    1。少操心：多一分权利多一分责任。资源（主要指你在程序中用到的各种object）控制权在你手里的时候你就多一分烦恼：用的时候去new 换的时候要修。现在资源控制权在第三方手里（spring框架），
    你程序只要开个口子，等人给你提供资源你用就行了。不用再考虑资源的管理（sigle responsibilituy）
    2。结偶，
    3。提高可维护性：程序写好从此用铁盒锁好，资源有问题现在比如要更换了，直接接口上换个新usb就行了。代码里面它再也不打开在里面改了（保持内部稳定，打开老修不经意间总会影响其他代码）
    4。资源被统一管理 效率提升
    5 可测试性提升

### DI 依赖注入
    IOC毕竟是一个概念嘛 那具体如何实现呢？你总不能纸上谈兵把？ 用DI
    eg：
    传统：
    public  Storre（）   {
     item = new ItemImpl1();
    }
    DI
    public  Storre（Item item）   {
     this.item = item
    }
    上面的例子可以看出来，用di 资源不由你管理 你只管接收 用。

# AOP
    @Aspect:使用这个的class都被认为是切面
    @pointcue：你的程序里的切点
    @aspect程序里的入点

     eg：
     Implement rate limit for querying weather information
      1. Add spring-aop
      implementation 'org.springframework.boot:spring-boot-starter-aop'
      2. Add @Aspect class and @EnableAspectJAutoProxy
        @Aspect
        @Component
        @RequiredArgsConstructor
        public class RateLimitAspect {..}

        @EnableAspectJAutoProxy
        @SpringBootApplication
        public class WeatherAppApplication {..}
      3. Create annotation interface
      @Target(ElementType.METHOD)
      @Retention(RetentionPolicy.RUNTIME)
      public @interface RateLimit {
      }
      4. define a method that contains the logic of the
      steps that need to be carried out when a method
      call gets intercepted
      public Object exceededLimit(ProceedingJoinPointjoinPoint) throws Throwable {..}
      5. Pointcut expression:
      Choose to use @Around annotation on our method with annotation interface
      @Around(“@annotation(RateLimit)”)



https://blog.csdn.net/L_GRAND_ORDER/article/details/112136702#:~:text=Spring%20IOC%20%28%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC%29%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0,IOC%20%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC%20%E5%9F%BA%E6%9C%AC%E7%90%86%E5%BF%B5%E5%B0%B1%E6%98%AF%E5%B0%86%E7%A8%8B%E5%BA%8F%E6%8E%A7%E5%88%B6%E6%9D%83%E4%BB%8E%E7%A8%8B%E5%BA%8F%E5%91%98%E6%89%8B%E4%B8%AD%E4%BA%A4%E7%BB%99%E7%94%A8%E6%88%B7%E8%87%AA%E5%AE%9A%E4%B9%89%EF%BC%8C%E4%BB%8E%E8%80%8C%E9%81%BF%E5%85%8D%E4%BA%86%E5%9B%A0%E4%B8%BA%E7%94%A8%E6%88%B7%E4%B8%80%E4%B8%AA%E5%B0%8F%E9%9C%80%E6%B1%82%E7%9A%84%E5%8F%98%E5%8C%96%E4%BD%BF%E5%BE%97%E7%A8%8B%E5%BA%8F%E5%91%98%E9%9C%80%E8%A6%81%E6%94%B9%E5%8A%A8%E5%A4%A7%E9%87%8F%E4%BB%A3%E7%A0%81%E3%80%82%20%E6%A1%88%E4%BE%8B



# 2 javaconfig
-相当于xml 它的替代 是一个java类
-可以创建类 放入到spring中
-用两个注解：
@Configuration 放在文件上面 意思是这个类是config 为什么配置类必须要用configuration呢？ 因为这个东西其实就是application.yml里面的东西，只是这个配置中有逻辑代码要写 比如拦截器 你如何拦截 拦截谁？这都是只有在class里用java才能写的东西 你写完后把它当作configuraion注入在了bean而已 
@Bean(name = "") 创建方法   返回值是对象 方法上用@bean 意思是把返回值对象注入到容器中 其中的name就是id 必须和类名一样
 -使用注解比xml文件方便
Application ctx = new AnnotationConfigApplicationContext(xxx.class)
XXX xxx = ctx.getbean("xxx")
-自动配置原理


# 3 @ImportResource
-作用：导入其他的配置文件，和当前文件合并为一块儿 作为配置使用
-使用：@importResource(value = "classpath:applicationContext.xml，classpath=“”")

# 4 以下是4种使用配置文件给类中赋值的方式
@PropertyResource： 
-你逼酱 你非要用不是application命名的配置文件 就用这个引入
-与value连用 
-首先要明白spring框架有ioc  所有对象由工厂创建   这里的vo里的类上都有component注解 就是说允许创建对象  这里的PropertyResource就是规定工厂在创建对象的过程中去PropertyResource后面规定的配置文件中找相应的值  componentsacn就是一顿扫描 把需要创对象的都扫出来
@value：

不逼酱  你用application 但是是properties   就得手动用value一个个弄：eg：@Value("${person.name}") 从配置文件取值    @Value("#{11*2}") spring表达式   @Value("true") 字面量
@configurationpropeties   

-不逼酱 用官方指定的配置文件 弄好后可以用yaml牛逼的地方 就是这个方法：属性绑定 不需要你一个个value赋值
-作用是将配置文件中配置的每一个属性的值映射到这个组件中 将对应的属性绑定 只有这个组件是容器中的组件才能用这个功能（@component：把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）； 
- @ConfigurationProperties最适用于所有具有相同前缀的分层属
-用于自定义配置项比较多的情况
-pretix前缀就是点前面的 比如 school.name
-如果想给自己的配置信息加上鼠标悬浮出现的描述信息的话要加一个依赖：spring-boot-configuration-processor
运行的时候写环境变量
-环境变量会覆盖yaml/proopetites配置

 
# 5 application propety和yaml
-property：server。port=8080
-yaml：server：
                         port：8080
-yaml注意缩进 可以在json formatter网站检查
-yaml格式
     person:
        name: 12
        list（数组）:
            - code
            - music
        map: {k1: v1,k2: v2}

# 6 resource自动注入
-在容器中去找对应类名的对像注入

# 7 多环境配置
-有开发，测试，上线环境三种主要的不同的环境，每个环境的端口，上下文件，数据库url， 用户名，密码都不太一样，这时就需要多环境配置
-使用方式：1 创建名称不同的多个配置文件：application-环境.properties/yml eg:application-test.properties
                       2 在application.properties文件中指定用哪个: spring.profiles.active=dev


# 8 目录结构



# 9 后端三层架构
-这里的dto和entity都是封装数据的  dto更多是处理前端来的json格式的文件  entity是数据库的  两个不相通





# 9 创建springboot方法
-start.springboot.io
-直接创建maven项目 导入父依赖和springweb依赖：然后在resource创建一个application.properties文件

# 10 @SpringBootApplication
-主要有以下三个：
     @componentScan：扫描本包以及子包下的注解
     @springBootConfiguration：当作配置文件用。也就是说可以吧bean注入到容器
     @EnbaleAutoConfiguration(starter的核心功能)：吧大多数要用的例如mybaits创建好bean 注入到容器中
                                                             维护外部的配置文件 ：application.property
Starter是启动依赖，它的主要作用有几个。
Starter组件以功能为纬度，来维护对应的jar包的版本依赖，
使得开发者可以不需要去关心这些版本冲突这种容易出错的细节。
Starter组件会把对应功能的所有jar包依赖全部导入进来，避免了开发者自己去引入依赖带来的麻烦。
Starter内部集成了自动装配的机制，也就说在程序中依赖对应的starter组件以后，
这个组件自动会集成到Spring生态下，并且对于相关Bean的管理，也是基于自动装配机制来完成。
依赖Starter组件后，这个组件对应的功能所需要维护的外部化配置，会自动集成到Spring Boot里面，
我们只需要在application.properties文件里面进行维护就行了，比如Redis这个starter，只需要在application.properties
文件里面添加redis的连接信息就可以直接使用了。

# 14 jsp使用
-导入jsp依赖：

-main下创建webapp目录用来存jsp 但是创建好后一般发现无法新建jsp 需要把main添加为外网资源目录才行

-指定jsp存放目录



# 15 手动获取容器
-ApplicationContext ctx = springapplication.run(Application.class, args)
   ctx.getbean("xxx")

# 16 js303校验
-类上记得加@validate


# 17 post和get请求区别
（1）post请求更安全（不会作为url的一部分，不会被缓存、保存在服务器日志、以及浏览器浏览记录中，get请求的是静态资源，则会缓存，如果是数据，则不会缓存）
（2）post请求发送的数据更大（get请求有url长度限制，http协议本身不限制，请求长度限制是由浏览器和web服务器决定和设置）
（3）post请求能发送更多的数据类型（get请求只能发送ASCII字符）
（4）传参方式不同（get请求参数通过url传递，post请求放在request body中传递）
（5）get请求的是静态资源，则会缓存，如果是数据，则不会缓存
（6）get请求产生一个TCP数据包；post请求产生两个TCP数据包（get请求，浏览器会把http header和data一并发送出去，服务器响应200返回数据；post请求，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 返回数据）


# 18 静态资源存放位置
-webjars  ：    localhost: 8080/webjars
-public/*      static/**         resources   
-优先级： resource》static》public
-注意：自己在配置文件里自定义目录后：spring.mvc.static-path-pattern=he 上面的就会失效


# 19 templates
-在template下的所有页面只能通过controller来跳转
-需要模版引擎thymeleaf的依赖

# 20 模版引擎thymeleaf
## 作用：  
模版引擎都放在服务器端，都是干差不多的事 就是把数据从后台取出来放到模版对应的坑里，然后把整个页面弄成html返回给浏览器
eg Hello ${xxx}>>>>>  Hello zhangsan
-template里的静态资源要通过controller跳转
-thymeleaf使用的时候要导入依赖 
-Thymeleaf特点：
动静结合： Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。
Thymeleaf支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。
开箱即用： Thymeleaf提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、改jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。
多方言支持： Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。
与SpringBoot完美整合，SpringBoot提供了Thymeleaf的默认配置，并且为Thymeleaf设置了视图解析器，我们可以像操作jsp一样来操作Thymeleaf。代码几乎没有任何区别，就是在模板语法上有区别。
————————————————
版权声明：本文为CSDN博主「龙源lll」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Lzy410992/article/details/115371017
## jsp和thymeleaf区别：
改变html页面风格（Let's change the page style）
假象我们写完了页面，但是我们突然想改变一下按钮周围的颜色：从绿色改为淡蓝色。我们不确定那种蓝色更加合适，因此我们需要多次尝试不同的颜色组合。
下面看一下采用JSP和Thymeleaf的实现方式：
采用JSP修改html页面风格（Changing the page style using JSP）
第1步：将应用程序部署到开发服务器上，并启动服务器。如果服务器不启动，JSP页面不会渲染，因此这一步是必须的。
第2步：导航到需要修改的页面。通常情况下，需要修改的页面是该应用程序的所有页面中某一个，很有可能需要点击多个链接、提交多个表单、查询多次数据库后才能跳转到这个页面。
第3步：打开firebug、dragonfly或其他嵌入浏览器的web开发工具。通过这些工具我们可以修改页面风格，并直接作用到浏览器的DOM上，并显示给我们。
第4步：修改颜色。尝试多种颜色组合后，确定最合适的颜色。
第5步：复制修改过的代码，并粘贴到对应的CSS文件中。
完成！
采用Thymeleaf修改html页面风格（Changing the page style using Thymeleaf）
第1步：双击.html模板文件，在浏览器中直接打开它。由于是Thymeleaf模板，它将会正常显示，采用模板/原型（template/prototype）数据（注意订阅类型选项）：
第2步：使用文本编辑器打开.css文件。模板文件在其<link rel="stylesheet" ...>标签中链接了CSS（Thymeleaf在执行模板时会把该标签中的href替换为th:href）。因此对CSS文件的修改会显示在页面中。
第3步：修改颜色。和采用JSP一样，我们仍然需要尝试多种不同的颜色组合，使用F5刷新页面。
完成！
重大区别（That was a big difference）
步骤数量的差别并不重要（在Thymeleaf的例子中也可以使用firebug）。真正重要的区别在于其复杂性，JSP方式中每一步骤需要花费的精力和时间。采用JSP，就必须部署并启动整个应用程序，这使得JSP不如Thymeleaf方便。
下列情形将进一步体现上述区别的意义：
开发服务器并不在本地，而是远程服务器。
不仅修改CSS，而且添加或删除了一些HTML代码。
还未实现页面所需的后台逻辑
最后一点是最重要的一点：如果应用程序仍在开发中，页面展示所需的Java逻辑还未完成的情况下，我们如何给客户展示新的颜色？（因为此时java代码没弄完 你jsp编译都过不了怎么展示？）（或者想让顾客选择颜色）...
使用JSP作为静态原型不行吗？（And what about trying to use the JSP as a static prototype?）
你可能会问，我们为什么要那么费事儿的修改JSP，像Thymeleaf一样直接打开JSP不行吗？
答案是：NO。
我们可以试一下：我们要把.jsp文件的后缀名改为.html，打开后结果如下图：
页面哪去了？其实页面还是那个页面。只是为了让页面以JSP的方式执行，我们要添加许多JSP标签和特性。但是改为HTML后，浏览器就无法正确的显示了。
再一次回顾一下双击Thymeleaf模板打开后的样子：
完全不是一个级别的了。。。

# 21 thymeleaf语法
概述
-所有静态资源都要被thymeleaf接管 只要在元素属性前面加th：即可
使用前要做的
-使用前必须在html上加上命名空间：xmlns:th="http://www.thymeleaf.org" 不然
下面的有时有thymeleaf语法都不生效（命名空间：允许你通过一个网址来识别你的标记。）
-使用前要把模版引擎的缓存关闭 不然可能不生效
语法
-@{/css/....} ：链接要用
-#{...}：国际化消息取：
-fragment：html页面中可以定义碎片：<th:fragment=“xxxx”>
碎片可以插入到别的地方：<div th:insert="~{哪个页面（去除html后缀，但要带路径）:: 哪个碎片}">
注意：可以把这些公共的放到一个compont页面里
-$: 取变量
-each: 遍历
 <tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
      </tr>

国际化
-webmvconguration里面有个localeResolver地区解析方法，里面大概意思是：
如果客户配置了就用客户配置的 没有就用默认的 我门需要自己配置 配置的时候
就是把http请求里面携带的lang值取出来，赋值给locael类就行
-resources下创建i18n文件夹
-i18ni下创建login.properties  和 login_zh_CN.properites 弄完后两文件自动合并
-添加英文的:login_en_US.properties(右击i18n也能创建)
eg: login.tip=请登录
-把login.tip绑定到html对应的地方
-页面左下角有个resourcse bundle按钮 点开可以可视化配置（同时配三个）
-application.properties里面配置：
-spring.message.basename=i18n.login
-想要实现点击自动切换 要自己在config配置自动化语言组件并将组件
配置到spring容器中（@Bean）


# 22 servelet和controller关系
Servlet是JavaEE技术中的一种，用于处理客户端发起的HTTP请求，接收客户端发送的参数，并返回响应的服务器端程序。
Controller是一种架构模式，它是MVC（Model-View-Controller）架构的核心部分，专门用于处理用户交互，接收用户输入，并将其转发给Model和View，最终将处理结果返回给用户。
Servlet可以实现Controller的功能，因此Servlet也可以被看作是一种Controller。

-servlet是为了页面可以动态展示数据 通过doget  dopost方法实现相应， 通过内嵌java代码实现动态展示
servlet和controller什么关系
-controller是mvc里的东西，表面上看功能就是servelet，但是单独拿controler
来说的的话它并没有做到网络层面的请求接受转发功能，spring框架中sevelt容器和spring容器是两个独立的部分，一起跑在web容器中（tomcat），请求会先到servelt 然后被servlet发到spring的dispatchservlet，然后才到controller


# 23 JSP和servelet的区别
-servelt是java代码里填html：jsp没出来的 时候servelet是在java代码中拼接html代码返回给浏览器，你想想乱的很么，也很难维护么，满屏都是print打印语句，你html语法哪儿错了还没法报错因为都是字符串
-jsp是html里填java：jsp出来了，使用jsp直接可以在html页面挖坑，坑里写表达式，servlet返回过来的值被填在坑里，然后吧填好的html返回给浏览器。其实jsp底层还是拼接，只不过这些脏活累活它帮你做了，你的重点就可以放在servelt逻辑上了

-一句话：JSP是代替servelet view角色的servlet
-JSP就是servelt的封装，只不过可以直接写在html里，就能实现数据动态绑定了
-JSP最后还是翻译成servelet了：翻译规则


# 24 spring mvc自动配置
-想要扩展mvc配置比如过滤器 拦截器 格式转换器等：创建实现webMvcConfigurer接口配置类 (@confugration)   不要加@enabklemvc
-自己想要写定制化的组件 实现对应接口写完后@bean交给spring管理  spring就会给你自动装配

# 25 lombok
-动态生成get set tostring。。。
-@Data   get  set

-@AllArgsConstructor    有参
-@NoArgsConstructor      无参

# 26 repositoty
@Repository注解修饰哪个类，表明这个类具有对对象CRUD（增删改查）的功能。
被@Repository注解的类可以自动的被@ComponentScan 通过路径扫描给找到（与它的来源有关）。
与@Controller、@Service、@Component的作用类似，都是把类交给spring管理。@Repository用在持久层的接口上，这个注解是将接口的一个实现类交给spring管理。
将所标注的类中抛出的数据访问异常封装为 Spring 的数据访问异常类型。
————————————————
版权声明：本文为CSDN博主「weixin_38218035」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_38218035/article/details/127237933

# 27 resource 和 autowired区别
-都是用来传值的 你传个类 它就去spring工厂找对应类的对应对象
因为spring'工厂你要东西的时候它就给你new好了 所以传的是对象
-resource默认按名称找  找不到按类型
eg：User cxc 此时会先去找和cxc同名的类 找不到 去按User找如果此时User接口被两个类实现了 就报错 解决方法是酱cxc换成其中一个实现的
autowired 正好相反 默认按类型注入
-resource是jdk里的 是java的东西 autowired是spring的 所以你想写更common的代码的话用resouurce


-使用autowired值为null
情况1：Bean对象并没有交给Spring管理
检查@Autowired的对象是否已经被注入到Spring容器中了；
确保使用@Autowired注解的对象也已存在Spring的容器中。
情况2：对象使用过new关键字
这是我遇到的情况，当一个对象使用过关键new时，它是不能被Spring所管理的。
所以如果在这些对象中使用@Autowired去注入对象，得到的结果也是为null

-autowired注入的类型如果有重复的怎么办：比如datasource 好几个bean都是这个类xing（mysql postgres。。。）
用qualify消除歧义qualify（value="x"）   用： qualify（"x"）


# 28 首页配置（想显示页面的初始化配置）
-静态资源托管给thymeleaf：你把路径手动改对了也能显示css这些，但是
就无法很好的动态匹配项目 就是有些自动匹配的东西你没法用 比如你弄了个下面👇说的虚拟路径，你的css访问有可能也会出问题。但是你把这些资源给thymeleaf这些事
它就会给你解决
-可以给项目加一个虚拟目录路径：
server.servlet.context-path=xxxx

# 29 Bean使用
-Bean的创建和使用
https://blog.csdn.net/liuyueyi25/article/details/83244239
### bean的初始化流程
    资源定位：@componentscan：，
    bean定义：将bean保存到beandefnition的实例中
    发布bean定义： IOC容器装载bean
    创建bean对象：实例化（你怕相应性能可以设置懒加载）
    依赖注入： autowired
### 作用域scope
    使用
    @Scope（WebApplicationContext。SCOPE——REQUEST）
    singleton：单例
    prototype：每次创建新的
    session
    

和component的区别
bean放在方法上，会把方法的返回值变成一个实例 一般和@configuration一起用（放在类上）
component 会把class变成实例存起来

为什么要用bean呢？
你想 你要用一个实体 但是某个实体比如passwordEncoder这么复杂的东西你没法自己写 人家已经写好了 但是人家的哪个类里肯定没有conpomennt 你总不能去动源代码吧？ 一个可行的方式是你先new出来 你再吧这个实体抛出去变成bean 其实和component
做的一样的工作 只是说这种情况下你想共享某个instance 你只有通过bean

# 30 controller接收请求参数的几种方法：@RequestParameter @RequestBody @pathvaiblaes @param（value="xx"）实体类 字符串
@Requestbody和@RequestParameter
Requestbody意思是取出post请求body中的数据 在变量前用这个意思是要把这个封装到指定的对象里
RequestParameter就是正常form中通过name传过来的值 
@pathvaiblaes：就是url路径里的值：@pathvariable(name="xxx")与@requestmapping（“/user/{xxx}”）中的xxx对应
@param（value="xx"）：用在变量前，就是给变量规定名字，经常在mapper（mybaties）中用到
@ResttController：相当于@ResponseBody和@Controller的结合。返回的是字符串
## 为什么要用requestparam接收参数而不是直接用变量？
写上reqeust这个注解就表示请里必须要有相关字段参数，如果没有就直接返回4xx的报错。4xx的报错意思是错误是用户自己导致的。如果不同，就不会有这种过滤功能。有没有参数都会进哪个controller方法。这对资源是一种浪费


参考：
https://blog.csdn.net/weixin_46141585/article/details/117391904

## 小坑：页面跳转后url不对
登录成功后重定向到某个页面 不然登录后url显示：xxx?user=xx,pws=xx
return "redirect:/xxx"

## 小坑：There was an unexpected error (type=Bad Request, status=400). Required String parameter 'username' is not present
-一旦我们在方法中定义了@RequestParam变量，如果访问的URL中不带有相应的参数，就会抛出异常——这是显然的，Spring尝试帮我们进行绑定，然而没有成功。但有的时候，参数确实不一定永远都存在，这是我们可以通过定义required属性：
@RequestParam(name="id",required=false)
//在参数不存在的情况下，可能希望变量有一个默认值
@RequestParam(name="id",required=false,defaultValue="0")
https://blog.csdn.net/qq_33840251/article/details/88774613

# 31 模拟数据库中的数据
-写对应的POJO类 把该有的字段都列出来
-创建一个POJO对应的Dao层做四件事：
    新建一个static map<Integer,POJO对象>的成员变量模拟数据库
    static方法中加入虚拟数据
    写一个获得所有数据的方法
    写一个通过id查对应数据的方法

-把Dao托管给spring：@repisitory（作用和component一样 专用于dao'）

# 32 拦截器
拦截器和过滤器的区别
拦截器是sprigmvc自带的 只是作用于controller层
拦截器怎么写
-在自己的配置文件里重写拦截器方法（implements HandlerInterceptor）
注意excludepathpatterens后面的是放行的

-在webmvncon类中把自己写好的拦截器注册到spring中：



# 33 get和post请求区别
1、 get是从服务器上获取数据，post是向服务器传送数据。
2、 get请求时通过URL直接请求数据，数据信息可以在URL中直接看到，比如浏览器访问；而post请求是放在请求头中的，用户无法直接看到。
3、 get传送的数据量较小，有限制，不能大于2KB；这主要是因为它受约于URL长度的限制。post传送的数据量较大，一般被默认为不受限制，但理论上，IIS4中最大量为80KB，IIS5中为100KB。
4、get请求因为数据参数是暴露在URL中的，所以安全性比较低，如密码不能暴露的就不能用get请求；post请求中，请求信息是放在请求头的，安全性较高，可以使用。
6、Get限制From表单的数据集的值必须为ASCLL字符，而Post支持整个ISO10646字符集。
说明：
1、get方式的安全性较Post方式要差些，包含机密信息的话，建议用Post数据提交方式；
2、在做数据查询时，建议用Get方式；而在做数据添加、修改或删除时，建议用Post方式；
以上就是get请求和post请求的区别有哪些？的详细内容，更多请关注php中文网其它相关文章！

# 35 @ResponseStatus   @Notblank @pattern
-REscontroller方法上用 指定返回状态
-Not类成员变量用 规定不为空  注意洗完后要用的话在引用变量前加@valide   但是在局部变量可以直接用！！！！
-pa：指定正则表达式

# 36 controller全局异常处理 
## 将异常暴露给后台
-controller包下新建ErrorResponse类 放在controller的原因是在这一层有response 可以返回友好的错误提示
-controller包下写异常处理类
@slf4j
@RestControllerAdvice
public class ControllerExpectionHandler{

@ExpectionHandler(Exception.class)
@ReasponseStatus(Httpstatus.INTERNAL_SERVER_ERROR)
public ErrorResponse hanlerExecption(Exception exception){
log.error(exception.getMessage(),exception);
return new ErrorResponse(500,exception.getMessage(),exception.getMessage())
}
}

-针对性处理exception：遇到哪个  复制哪个 处理哪个 （@exceptionhandler后面加入哪个class） 
还可以指定你想返回的状态码

可以循环取出同类异常下的所有异常


## 将异常暴露给前台

# 37 IOC
Spring Ioc容器
实现控制反转主要是BeanFactory和Applicationcontext两个接口。
BeanFactory
BeanFactory是一个管理Bean的工厂，该类会根据XML配置文件来装配Bean。
ApplicationContext
ApplicationContext是BeanFactory的子接口。创建ApplicationContext实例的方法有三种。


# 38 bean和component区别
@Component： 作用于类上，告知Spring，为这个类创建Bean。
@Bean：主要作用于方法上，告知Spring，这个方法会返回一个对象，且要注册在Spring的上下文中。通常方法体中包含产生Bean的逻辑。 相当于 xml文件的中<bean>标签。


# 40 修改和删除user
html里
<a th:href="@{/delete/}+${user.getUsername()}">
   <button type="button" class="btn btn-primary">delete</button>
</a>
controller里
@RequestMapping("/delete/{username}")
public String deleteUser(
        @PathVariable("username")
        String username
){
    userDao.deleteUser(username);
    return "redirect:/emps";
}


# 41 404 500....页面
-template下弄一个error文件夹 里面放个404.html 500.html 包相关错误会自动倒过来

# 42 注销
-session.invalidate()

# 43 注意
-contiroller别存临时变量  比如定义成员变量private map 吧来的request存到里面干个啥 这会导致很多问题 因为它会把每个reques都存 可能会引发一些问题

# 44 RESTful 架构
综合上面的解释，我们总结一下什么是RESTful架构：
　　　　（1）每一个URI代表一种资源；
　　　　（2）客户端和服务器之间，传递这种资源的某种表现层；
　　　　（3）客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

作者：AWeiLoveAndroid
链接：https://www.jianshu.com/p/84568e364ee8

# 45 RESTful feature：
## resources
-unserstandability:前后端都能懂
-completeness：一个url对应一个完整的资源（一个资源包含一些其他资源）
-linkbility: 前后端链接

## message：    
http request/response



## address：  
url（urL对应资源的那部分）定义规范
-用名词复数形式
-用小写字母
-向后兼容性：
get：如果你要变，不能直接改uri，因为这样前端也要改，可以重定向到新地址
post：把属性弄成optional  或者用版本eg：http://localhost:8080/api/v1/users/1

## method
post get put delete options

## statelessness
-idempotency密等性：相同的请求返回相同的结果  存钱50的请求发出去永远返回你余额50

## caching
作用
减少IO操作 提升performace
caching

分类
-client caching：不用发请求了
-CDN caching：存一些图片啥的
-server caching(redis spring自带caching)：就不用老去访问数据库了

# 46  openapi swaggerapi
作用
写代码的时候通过相关注释等直接把文档要的内容弄好了 使用这些文档依赖就可以直接倒出
关系
swagger维护openapi
怎么用
1.直接导入open API依赖 它会自动部署swagger-ui
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.0.2</version>
</dependency>
2. 路径后加 /v3/api-docs  访问openapi
3. 路径后加swagger-ui.html 访问swagger、

api上怎么加描述
方法上加 @Operation(summary = "xxx")


# 47 error: src refspec main does not match any.
error: failed to push some refs to 'git@github.com:ShaunChang/hello-world.git'
-本地分支与远程分支不同名或没有关联 

# 48 过滤器
步骤一：自定义过滤器
public class MyFilter implements Filter{
@Override
public void doFilter(xxxx){
xxxxx
filterChiain.doFilter(serverletRequest,servletResponse)
}
}
步骤二：把过滤器加入容器中
@Configuration
public class WebApplicationConfig{

@Bean
public FilterRegistrationBean filterRegistrationBean(){
FilterRegistrationBean bean = new FilterRegistrationBean();
bean.setFilter(new MyFilter());
bean.addUrlPatterns("/user/*");
return bean;
}
}


# 49 aoplication配置字符编码
server.servlet.encoding.charset=utf-8
server.servlet.encoding.force=true

# 50 mybaits 操作mysql
## 步骤
### 1 加入mybaits起步依赖
选mybatis framework和mysql driver两项
要么手动添加：
```java
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>  
    </dependency>   
```



### 2 pom.xml文件制定把java目录中的xml文件包含到classpath中
目的是使所有的xml文件都被放到target下的classes目录中，这样程序运行的
时候就有这些xml文件
```java
<resources>
<resource>
<directory>src/main/java</directory>
<includes>
<include>**/*.xml</include>
</includes>
</resource>
</resources>
```


### 3 创建实体类
-pravite属性 @Data
### 4 创建Dao接口及对应的增删改差方法
-创建StudentDao类
-创建接口方法：Student selectById(@RequestParam  Integer id);
-让项目识别mapper
第一种方法
在类名上加@Mapper告诉myubatis这是dao接口 在 Spring 程序中，Mybatis 需要找到对应的 mapper，在编译的时候动态生成代理类，让代理类去做链接数据库等那些冗余的脏活累活，实现数据库查询功能，所以我们需要在接口上添加 @Mapper 注解。
这里
第二种方法
```java
@mapper你嫌麻烦你可以在application类上写mappersacn 一下全扫描出来
@SpringBootApplication
@MapperScan("com.scut.thunderlearn.dao")
public class UserEurekaClientApplication {
public static void main(String[] args) {
SpringApplication.run(UserEurekaClientApplication.class, args);
}
}
```

### 5 创建接口对应的mapper文件 xml文件 写sql语句
-创建同名.xml文件
```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="dao接口的全限定名 可以右建复制">
<insert id="方法名称" parameterType="要返回的类的全限定名">
</insert>
<select id="" resultType="">
select id,name,age, from student where id=#{dao中绑定的那个值}
<selcet>
</mapper>
```

### 6 创建service接口以及实现类
-创建service接口并定义一个方法不用煮食
Student queryStudent(Integer id);
-创建serviceImpl方法实现接口 用@serviece注释
1 引入要用的Dao接口：@Resource  pravite xxxxDao xx
2 重写方法：
Student student = studentDao.selectById(id)
return student
### 7 创建controler对象访问service
第一种方法
-@Resource private xxxService xxx
第二种方法
@@RequiredArgsConstructor： 用这个的好处是可以在运行前检查出潜在的循环依赖问题
1.必须声明的变量为final
​ 2.根据构造器注入的，相当于当容器调用带有一组参数的类构造函数时，基于构造函数的 DI 就完成了，其中每个参数代表一个对其他类的依赖。基于构造方法为属性赋值，容器通过调用类的构造方法将其进行依赖注入
### 8 写appplication,properties文件 配置数据库的连接信息
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springdb?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=
# 51 dao和mapper分开管理
-吧xml文件放到resource下的mapper包（自建）下
-application.properties: mybatis.mapper-locations=classpath:mapper/*.xml
-pom: resource: directory:src/main/resources  include: **/*.*
# 51 mabites 日志log
-applciation.properties: 
mybatis.configuration.log-impl=org.appache.ibatis.logging.stdout.stdOutImpl
# #52 事务 没学完
springBoot用什么处理事务
使用事物管理器接口（DataSourceTransitionManager）管理事务，有不同实现类比如jdbc或mybatis
如何设置事务一些属性
-使用申明式事务，就是在xml文件配置：隔离级别，传播行为，超时时间
事务怎么用：
springboot框架
在业务方法上加@Transactional，此方法就具备了事务功能
在主启动类上加@EnableTransactionManager
aspect框架
在xml文件中配置事务控制的内容

# 53 项目依赖
    lombok
    Spring web
    spring Data JPA
    H2 database
    PostgreSQL Driver
    Validation
    Flyway Migration
    Spring boot Actuator

# 54 mapper文件
## 目的
做entity和dto之间的转化。比如把user转化为其他dto。。。。
## 如何使用
### 传统
-创建UserMapper类   
-类上加@component：不加就无法在service这样用：private final UserMapper   
-在service里引入：private final UserMapper
-UserMapper里面写上usermapperToxxx  UserMapperToxxx这些方法，方法里面就是最笨的那种写法：xxx.setxx(user.get(xxx))
### Mapper插件
-导入依赖：
implementation 'org.mapstruct:mapstruct:1.5.2.Final'
annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.2.Final'
-定义userMapper接口,里面规定方法名，传入值，转出值即可
```java
@Mapper(componentModel="spring")
public interface UserMapper{
    UserGetDto mapperUserToUserGetDto(User user);
}
```
-转换过去后变量名不一样的解决办法：在方法上加：
@Mapper(source = "xxx",target = "xxx")
-转换过去后数据类型不一样的解决办法：在mapper后的uses属性中添加那个缺失的数据转换
@Mapper(componentModel="spring"，uses="UserMapper.class")

### builder
在要转换的类上加@builder
然后在转换类里写：
```java
 public PropertyGetDto mapperPropertyToPropertyGetDto(Property property){
        User user = property.getUser();
        UserGetDto userGetDto = userMapper.mapperUserToUserGetDto(user);
        PropertyGetDto propertyGetDto = PropertyGetDto.builder().typo(property.getTypo()).userGetDto(userGetDto).size(property.getSize()).id(property.getId()).build();
        return propertyGetDto;
    }
```


# 55 为何能使用private final xxx代替@Resource
原因
注意文章的第一句话有构造方法的Bean是可以通过private final来引入的，而上述的Controller中是加了@AllArgsConstructor注解的，它会给这个类加上全参的构造函数，这样就会在该类初始化时同步给属性初始化，也就是调用各引用Bean的初始化方法
因此就可以使用private final的形式来进行引用bean了
注意同一个类不要使用两种引入方式，要么用private final，要么用@Autowried


# 56 unit test
## 后端有哪些test
    unit test：单元测试（Unit Test）是指对软件中的最小可测试单元进行检查和验证。例如，在一个web应用程序中，可以将每个函数作为单元，并对每个函数进行测试，以确保它们在各种边界条件下正常工作。
    integration test： 集成测试（Integration Test）是指将单元测试合并在一起，以检查它们之间的交互是否正确。例如，在web应用程序中，可以测试不同的模块之间的集成，以确保它们正确地一起工作。
    end to end test：端到端测试（End-to-End Test）是指将集成测试与实际用户操作结合起来，以检查实际用户在应用程序中的操作是否正确。例如，在web应用程序中，可以模拟真实用户行为，并检查在用户输入数据
## 什么是unit test
前端来说一个组件测试，后端来说一般指一个类的测试
## unit test的意义是什么或者说为什么不直接弄个class在里面写测试
你想想，现在比如要测试dao文件，如果不用单元测试，那势必会执行dao中的数据库操作，如果此时你的数据库没有启动起来，首先这一步你就错了，后面的dao的结果肯定错。你的工作重心是在dao层的代码逻辑，而你的测试却是由一个不相干的数据库的问题而导致了失败。很冤枉。所以在unit case中所有的依赖部分的代码和数据可以自己模拟，那个方法其实压根就没有执行。一句话，单元测试目的是排除干扰。
## service等普通类测试步骤
1 类上加@ExtendWith(MockitoExtension.class)  
2 用@mock引入测试此类中用到的额外的成员变量  
3 用@InjectMocks注解引入你想测试的类 
4 import static org.mockito.Mockito.*;  import static org.junit.jupiter.api.Assertions.assertEquals;
5 测试方法
其中写when 。。thenreturn中写的都是你要测的那个方法中用到的外部方法，这些方法在上面已经被mock过了，所以是假的，代码走到那儿的话不会执行，你要模拟执行一遍，返回假数据。那么为什么要把这些mock呢？直接用不就行了。原因是为了保证这个测试的纯粹。本测试不想测试这些依赖方法，就用mock的方式模拟返回数据

```java
 @Test
    void getUser() {
            User user = new User();
            Long userId = 1L;
            user.setId(userId);
            user.setPassword("11111");
            user.setName("cxc");
            user.setEmail("943294932423");

            UserGetDto userGetDto = new UserGetDto();
            userGetDto.setEmail("943294932423");
            userGetDto.setName("cxc");

            when(userRepository.findById(userId)).thenReturn(Optional.of(user));
            //注意这里是userRepository.findById(userId),不是service.findUser方法 因为@mock的是注意这里是userRepository
            when(userMapper.mapperUserToUserGetDto(user)).thenReturn(userGetDto);
            assertEquals(userService.getUser(userId),userGetDto);

            }
```

## mockMVC
1 成员变量加入mockMVC：  
private MockMvc mockmvc;  
2 在@BeforeEach方法下初始化mockmvc 可以只通过自己要测的那个类初始化 这样加载的资源就少环境相对干净  
写法一：
类名上加springbootest注释
@BeforeEach
void setup(){
 mockmvc = MockMvcBuilders.standaloneSetup(new 你要测的类名()).build();}  
 写法二： 在测试类名上加@WebMvcTest(xxxx.class) 然后在下面直接autowired mockmvc beforeeach也不要了   
3 在测试类中用perform模拟请求然后用andExpect测试是否符合预期：
```java
@Test
void xxx(){
get请求：mockmvc.perform(MockMvcRequestBuilder.get("controller  url"))
post请求：mockmvc.perform(MockMvcRequestBuilders.post("url").content(asJsonString(new User(xx,"xxx","xxx"))).contentType(MediaType.APPLICATION_JSON).accept(MediaType.APPLICATION_JSON))
.andDO(print())
.andExpect(status().isok())
.andExpect(content().string("xxx"))}
.andExpect(jsonPath("$.id").value("xxx"))   就是测响应的json里具体某个属性的值
```
小坑一：applciation starter failed ：找不到。。dao 原因是没有把所有要的类扫描 记住这是一个“干净”的环境   
解决方法一：@WebMvcTest({IndexController.class,UserDao.class}) .  
解决方法二：.put("/articles"+id)这里的id为什么一定要用拼接的写法
使用拼接的写法可以让服务器端能够准确的识别出请求的资源，从而可以做出准确的响应。为什么不能这样写.put("/articles/5")，因为服务器端无法准确的识别出请求的资源，如果有传入参数，服务器端无法识别出这个参数是什么，也就无法作出准确的响应。
小坑二：
```java
@ComponentScan(basePackages = {"com.tobacco.casemanagement"})
class IndexControllerTest {
    @Autowired
    private MockMvc mvc;

    @Test
    void loginController() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/login?username=cxc&password=123"))
                .andDo(print())
                .andExpect(status().isOk());
    }
```


# 57 @Entity @Table(name = "xx")
@Entiry：这个类被映射为数据库中的同名表 默认为类名
```java
@Getter
@Setter
@Entity
public class Property {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String typo;
    private Integer size;

    //这里你想直接显示owner id对应的哪个user 就得在代码里接着查一步 告诉代码依赖关系以及根据什么查
    @ManyToOne
    @JoinColumn(name = "owner_id")
    private User user;

    @CreationTimestamp
    private OffsetDateTime created_time;
    @UpdateTimestamp
    private OffsetDateTime updated_time;
}
```
@Table：类名与表名不同时通过这个注解指定

# 58 状态码
## 删除
@ResponseStatus(HttpStatus.NO_CONTENT)
## post
@ResponseStatus(HttpStatus.CREATE)
## 其他的暂时没看到

# 59 联表查询两种方式、
## 推荐方式二 更加oop 且所有方法都在自己service里 不需要跨类调动 更加结偶
## 方式一 调用对应的service层查询
比如要通过userid查对应的property。在usercontroller调用propertyservice的findPropertubyUseriid方法
```java
public List<PropertyGetDto> findPropertyByUserId(Long userId){
        List<Property> byUser_id = propertyRepository.findByUser_id(userId);
        List<PropertyGetDto> propertyGetDtos = byUser_id.stream().map((property) -> {
            return propertyMapper.mapperPropertyToPropertyGetDto(property);
        }).toList();
        return propertyGetDtos;
    }
```
## 方式二 在entity表中就一步到位让它自动查好给你放那儿即@ManytoOne
user entity中
```java
    @OneToMany(mappedBy = "user")
    private List<Property> propertyList;
```
property entity中
```java
    @ManyToOne
    @JoinColumn(name = "owner_id")
    private User user;
```
userService方法
```java
public List<PropertyGetDto> findPropertyByUserId(Long userId){
        User user = findUser(userId);
        List<Property> byUser_id = user.getPropertyList();
        List<PropertyGetDto> propertyGetDtos = byUser_id.stream().map((property) -> {
            return propertyMapper.mapperPropertyToPropertyGetDto(property);
        }).toList();
        return propertyGetDtos;
    }
```


# 59 SQL
### 数据库怎么设计
### data persistence 方式有哪些
    放在文件里 好处：删除方便 
    放在数据库里 好处：提供很多数据处理的方式
### 为什么要data persistence
    你把数据弄好不存下来你下次访问啥？傻逼
### 为什么要用数据库
    控制数据冗余：也不一定 还有时候会用冗余换performance
    data sharing        
    可以对写入进行约束
    访问限制
    backup和recovery
### 怎么设置主建
    自增
    与业务无关：1 数据安全，比如你用名字+地址为主建 数据有泄漏风险，2 业务变不会影响主建
    唯一性
### 怎么设置外建
    主键相关数据设置软删除功能（is——deleted字段），避免防止主键数据删除导致外建找不到数据的情况发生
### 什么是索引
    把某个字段或者某几个字段的数据按某种方式（索引）排序，比如name上你弄索引，比如啊 人家按照字母顺序排序了，可能这儿的索引就是a b c。。。这个东西。下次你要查name是ash的帅哥。以前的方法是一个 一个找，笨笨的 假如你数据有几十亿，小心给你累死。现在有索引了，一看，妈的字母a开头，就去字母a那一团数据里找ash
### 怎么设置索引
    在mysql当中，主键上，以及unique字段上都会自动添加索引的！！！！
    什么条件下，我们会考虑给字段添加索引呢？
    条件1：数据量庞大（到底有多么庞大算庞大，这个需要测试，因为每一个硬件环境不同）
    条件2：该字段经常出现在where的后面，以条件的形式存在，也就是说这个字段总是被扫描。
    条件3：该字段很少的DML(insert delete update)操作。（因为DML之后，索引需要重新排序。）

	建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。
	建议通过主键查询，建议通过unique约束的字段进行查询，效率是比较高的。
### 如何设计user数据库
    一些场景以及对应的商业上的用法
    
    如何设计user 和role数据库
    user role group 这样的话一个用户可以同时有多个group 而且改的话也好改 比如你现在要增加一个role 给大多数满足条件的用户都加上这个role，你用户有2000000000你要一个一个去加吗？
    你关联到group你就只需要把new role加到group就行了
    
    如何获得like 等数量大的信息
    userlike和article多对多
    如果用户有十万 article十万 10x10 中间表就会非常庞大
    所以这个表不能实时查询，可以用cache缓存。比如你要like量，热度等信息 中间表很大由于 所以： 用一个固定的时间去更新这个信息，保持最终一致性。但更新后每次新来的我就把那个cache加1 同时也把这个数据存起来

# 60 postgres
# 61 docker.component.yml
### 模版
    version: '3.7'

    services:
    postgres:
    image: postgres:14.2-alpine
    volumes:
    postgresql_data:/var/lib/postgresql/test/data/
    restart: always
    ports:
        15432:5432
    environment:
        POSTGRES_DB=weather 
        POSTGRES_USER=postgres
        POSTGRES_PASSWORD=admin
    networks:
        persist

    pgadmin:
        image: dpage/pgadmin4
        volumes:
            pgadmin-data:/var/lib/test/pgadmin
            restart: always
        ports:
            18002:80
        environment:
            //用浏览器打开paamdin 用下面的登录
            PGADMIN_DEFAULT_EMAIL: admin@test.com
            PGADMIN_DEFAULT_PASSWORD: admin
        networks:
            persist

    graphql-engine:
        image: hasura/graphql-engine:v2.9.0
        ports:
            "18080:8080"
        depends_on:
            "postgres"
        restart: always
        environment:
            // postgres database to store Hasura metadata
            HASURA_GRAPHQL_METADATA_DATABASE_URL: postgres://postgres:admin@postgres:5432/weather
            // this env var can be used to add the above postgres database to Hasura as a data source. this can be removed/updated based on your needs
            PG_DATABASE_URL: postgres://postgres:admin@postgres:5432/weather
            // enable the console served by server
            HASURA_GRAPHQL_ENABLE_CONSOLE: "true" # set to "false" to disable console
            // enable debugging mode. It is recommended to disable this in production
            HASURA_GRAPHQL_DEV_MODE: "true"
            HASURA_GRAPHQL_ENABLED_LOG_TYPES: startup, http-log, webhook-log, websocket-log, query-log
            // uncomment next line to set an admin secret
            // HASURA_GRAPHQL_ADMIN_SECRET: myadminsecretkey
        networks:
            persist

        volumes:
        postgresql_data: {}
        pgadmin-data: {}
        networks:
        persist: {}

### docker.component.yml文件中postgres.port字段xxx:xxx冒号前后数字是什么意思
    xxx:xxx冒号前后数字是指定容器内Postgres数据库的端口号。比如：5432:5432，
    表示容器内的Postgres数据库的端口号为5432，并且可以通过这个端口从本地主机访问Postgres容器中的数据库。
    也可以理解为给localhost暴露了一个端口，这样一来本地也可以访问那个postgres，比如在浏览器localhost:xxx就可以进入postgres/pgamdin之类的东西

### pgadmin用localhost装的话怎么配置？
    上面的配置是把pgadmin和postgres都弄在docker里面
    你要使用本地的，pgadmin创建链接的时候host填localhost。它这个写法是基于docker容器的，意思就是在docker容器里找这个东西

### pgadmin创建链接的时候hostname是什么
    你用localhost，名字就是localhost
    你用docker里面的服务 名字就是那个服务的名字 比如postgres

# 60 关系型数据库和非关系型数据库的区别是什么
    关系型数据库是一种基于表结构来管理数据的数据库，表中的数据以行和列的形式存储，其中每一行表示一条记录，每一列表示一个字段。关系型数据库运用SQL语句来定义、存取和处理数据。
    
    非关系型数据库不需要遵循表结构，其数据以文档、图像、视频等格式存储，不需要定义表结构和字段，可以存储各种格式的数据，并且可以动态地添加新的字段。非关系型数据库主要运用非结构化查询语言（NoSQL），如MongoDB、Redis等来查询和处理数据。

# 61 JPA

### ORM(objefct relational mapping)：
    解决oop与关系型数据将不匹配的技术，数据取出来封装为对象，对象存进去转化为数据

### 没有ORM的世界
    手写sql。逐表逐字段陈操作。eg：在dao中做很多读取对象数据，改变状态等重复操作

### 常见ORM
    herbinate：宣称不用写一行sql
    mybatis：动态sql见长

### 缺点
    转换耗CPU，所以在高性能需求的时候不用ORM

### 什么是JPA（java persistence API）
    是一套规范。主打定义，比如告诉你ontomany的时候依赖哪个字段转换，是老板，告诉你怎么做
    ORM是打工的，而ORM是一种实现JPA的技术。主要为了简化持久化开发和整合ORM技术

### 如何使用JPA
    添加依赖
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.flywaydb:flyway-core'
	runtimeOnly 'org.postgresql:postgresql'

    application中配置相关信息，让项目找到数据库，jpa等
        spring:
        datasource:
        driver-class-name: org.postgresql.Driver
        url: jdbc:postgresql://localhost:15432/postgres?currentSchema=weather //不同环境这三个会被重写
        username: postgres
        password: admin
        flyway:
        enabled: true
        schemas: weather
        
        jpa:   //做数据迁移用
        properties:
        hibernate:
        default_schema: weather
        jdbc:
        time_zone: UTC
        show-sql: true

### 快速上手weather实例
    @Entity //告诉这是一个希望映射的class 个人想法：把这些带entity的class'集合在某个地方'，当数据转换开始的时候，相关的工具（orm）回去这个地方找它的目标class 从而进行转换 你不放这个人家可能转换的时候找不到这个类
    public class Weather{
    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)//自增
     //不规定name，自动认为是下面的id  其他的设置其实就是@notnull 这些 看个人习惯 只不过notnull是lombok 注意不要把混用lombok的data和jpa
    private Long id；
    }

    @Repository 
    public interface xxxRepository extends JpaRepository
### JPA使用注意：
    不要把混用lombok的data和jpa
    用@Builder的时候一定加@NoargsConstructor

### 自定义简单查询
    findxxBy queryXXBy 可以加一And Or关键字  约定大于配置，你把名字按约定写好，参数按约定规定好，人家自定帮你写sql
### 自定义复杂查询
    加以下注解：@transiction（timout=10） 事务
              @modifying： 涉及到修改和删除就加
              @query（"updata User set username=?1where id=?1"）  这里到updagte后跟的是object 不是表名
### 多表链接查询
    @onetoone 
    @joincolumn(name="cxc", referencecolumnname="id")
    privat User user
    上述例子中joincolomun中的name是指表中原本有个字段叫cxc 去用这个当外键查user中字段id和cxc值相等的那个user 放在我这儿

### @Transient
    如果你想在类中加入某个变量。使只不与表任何字段关联，只是想拿出来做计算 就用这个
    怎么用
    1 定义这个变量并加注释
    2 写getxxx方法。里面就写上如何形成它
### lazy懒加载
    为什么有
    a表1000条数据 b表100000条数据 a有b外建 a查一次原本扫描1000即可 现在好了 由于没条a有个b 查到这块的时候还得去b搜匹配外建的那条 总查询1000x100000次 但是你数据又不用 浪费。。。

    @onetomany 默认lazy：查出来的东西很多 你有时候又不用 资源浪费 默认不查 用的时候再查
    @mangytoone 默认eager 可以修改：@Manytoone (fetch=FetchType.LAZY)
    @manytomany 默认lazy

### 事务
    1 放在service 不然每个reposityory都有事务 会发生数据不一致的情况 因为如果在repostiory上加事务 service
    对这个repository就会失去控制 
    2 在本类调用事务失效 比如在service写一个updateBanlance方法 你在本类的某个方法中使用this.updateBanlance,事务会失效。所以一定要在外面调用 要让调用 通过 （go through）它。
    意思是：事务就是给整个类加了事务外壳 你在里面搞 这个壳子用不上 要在壳外面搞 壳才会保护你 你他妈刀都放里面去了 穿防弹衣有m用
    3 不要在private上加事务


# 63 weather app
### 为什么创建自己的数据库，数据从api来不就行了？
    这种openapi一般都会有限制访问次数之类的，然后天气数据也不会更新很频繁，为了不让第三方server（openapi）影响我们的server，自己存数据
    
# database migration： 正确数据库操作
    为什么要
    真正的开发的时候比如删除某个数据或table的时候很危险，弄错会毁了。自己本地毁了没事，你把坏数据库迁移到其他比如test环境去就会有问题

    如何使用
    1 implementation 'org.flywaydb:flyway-core'
    2 ducoker compoennt: spring: flyway:enabled: true schemas: weather
    3 启动项目

    注意
    不要手动子啊pdamin加table 不然会启动报错，
    创建好后千万不要动flyway文件：哪怕格式都不行 因为底层用的是hash 你改一点点都会不一样 启动会报错


# 64 后端分页pageable
    repository
    Page<Property> fundByUer_Id(Long userid,pageable pageable)
    为什么返会page而不是list呢？ page多了两个方法前端展示可能能用到： getTotalElement getTotalPages
    
    service
    Pageable paging = PageRequest(page, size)
    Page<Property> propoties = propertyRepository.fundByUer_Id(Long userid,pageable pageable)
    List<Proteties> properityList = propertis.getContent()

    PropertyPageDto
    、

# 65 Spring security
    流程
    请求会先来到filterchain 👇下面会写相关的文件 里面就是一层层的filter 走完才到controller

    意义：
    1。让程序员专注于代码逻辑 验证安全这一块集中在secutiy弄
    例子：login这些验证为什么要用security写，自己去数据库查，一样放行不就行了？
    有时候诸如login的验证不光会验证密码 还会验证你是否身份有效之类的 当然我们可以自己去写。多写几个字段。那样你的程序扩展和维护性就很差。你login的逻辑就会穿插很多安全相关
    的代码。哦耦合度高。所以才有了security 它帮你把这些与login无关的验证放在前面，后面不管你是想添加验证还是修改啥的都会方便很多 你login的代码就不用动了
    例子：你刚去公司 不懂公司鉴权和验证等逻辑 如果没有security 你写相关controlle是不是需要考虑这些？你有得去学 开发成本高 
    2 由于约定大于配置，spring security事先已经帮你加了7 8 层security


    怎么用
    1. 添加依赖
    compile "org.springframework.boot:spring-boot-starter-security"
    2. 写Securityconfiguration文件：文件
    @EnableWebSecurity
    @Setter   是为下面的环境变量赋值而写的
    @Configuration
    @ConfigurationProperrties（prefix="management.endpoints.web.cors"） 比如下面的allowedOrigins在不同环境科恩跟不一样 所以把这中变的东西放在外部的配置文件中。结偶。
    public class securtiyConfiguration{
        @Bean
        public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
        .csrf().disable()  //csrf攻击是什么，举例说明 CSRF（Cross-site request forgery）跨站请求伪造，是一种恶意攻击，它利用用户已有的身份验证凭证，以用户的名义发送恶意请求。
                            例如：用户A已登录了某个网站，此时，攻击者发送一封恶意邮件给用户A，邮件中含有一个网址，当用户A点击此链接时，攻击者就可以以用户A的名义发送恶意请求，这就是一个CSRF攻击。
        .cors().configrationSource(request -> { CORS（跨源资源共享）是Spring Security中的一个特性，它允许来自不同源的资源（如JavaScript、图像等）在Web浏览器中进行跨源请求。如果不使用CORS，浏览器将不允许发送任何跨域请求，但攻击者仍然可以发起这样的请求，而不受任何限制。
            var cors = new corsConfigratino(allowedOrigins);
            cors.setAllowedMethods(allowedOrigins);
            cors.setAllOrigins(allowedOrigins)
            cors.setAllowedHeaders(allowedOrigins);
            cors.setExposeHealder(allowOrigins)
            return cors;
        })
        .addFilter(filer)
        .authorizeRequests()
        .antMatchers( "/**").permitAll()
        .anyRequest().authenticated()
        .and().build();
        }bran
    }
    3. 密码明文处理: 用BCrpt spring已经有对应方法了
    写Securityconfiguration文件：
    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCrptPasswordEncoder()
    }
    4. 注册
    service文件中的create user  ：
    。。。。
    public final PasswordEncoder passwordEncoder;
    public void createUser(UserPostDto userpostdto){
        userPostDto.setPassword(passwordEncoder.encod(usepostDto.getPassword())）
        userPostDto.save(userMapper.mapUserPostDtoToUser(userpostdto)
    }
    5. 密码验证
    passwordEncoder。match（userPostDto.getPassword(),数据库查的password）


    日常要掌握的一些security的一些东西
    1 Role-base access control: 可以在user和role之间加一个group
    2 https和http的区别 post的body'参数https会加密
    3 aviod sql injection
    4 no plain text for any password
    三大Authorisation方案：
    5 web登录状态保存：用户名密码鉴权；鉴权结果的保存在server的session里。session一般就是在服务端实例的memory里 即一个server一个session。如果有多个server的话会有风险：你的请求分发被load banlance管。请求不一定会到存了session的那台server。解决方案一般是在load banlance和多个server之间加一个shared redis 解决方案二是请求携带token,这样reids那一层就不需要了
    6 第三方API使用：Oauth鉴权，就是我们常见的那些用微信登录之类的 好处是不需要注册用户名密码 客户流失率小 。流程是client先去验证服务器验证，拿token 。然后带着token去访问资源
    7 安全信息传递 token鉴权： jwt。好处是你可以在里面加任何字段 比如过期时间 role这些 token是被加密的 一般放在header里面 可以安全传递。
    8 认证Authentication：
        怎么用
        (1)自己写一个filter然后把它add到filterchain里面 extends UsernamePasswordAuthenticationFilter{ //这里选择继承UsernamePasswordAuthenticationFilter的原因是这个filter里已经配置好了一些东西 它默认filter login路径来的POST请求  
            @override
            @SneakyThrows
            public Authentication attemptAuthentication(Request request, Response reponse) Throw AuthenticationExceprion{
                LoginRequest loginrequest = new ObjectMapper(request.getInputStream, LoginRequest.class); LoginRequest是自己写类就一个username和password属性 getter setter方法。objecetmapper用于把json字符串变成对应的objecet
                UserAuthenticationToken userAuthenticationToken = new UserAuthenticationToken(loginrequest.getUsername(),loginreqeust.getPassword())
                return AuthenticaitonManager.authenticate(userAuthenticationToken)
            }
            successfulAuthentication
            unsuccessfulAuthentication
         }
        (2)自己写一个ApplicationUserDetailService implemetnes UserDetaileService{
            private final UerRepository userreposity;
            @Override
            public UserDetails loadUserByUsername(string email) throw UsernameNotFoundException{
                 UserInfo userinfo = userRepository.findByEmail(emil).onElseThrow(()->new ResourceNotFoundException("User"+ emial)); 这里注意比较tricke的一点是 你的user entiry类最好改个名字 因为spring secruity里面已经有了一个user了 你放在前面接收会自动被认为是人家定义好的那个user
                 return new User(email, userinfo.getPassword(), Collotion.emptyList())
            }
        }
        （3）在SecurityConfiguration里面写public AuthenticationManager authenticationManager(){
            DaoAutheticationProvider daoAutheticationProvider = new DaoAutheticationProvider();
            daoAutheticationProvider.setUserDetailService(ApplicationUserDetailService)
            daoAutheticationProvider.setPasswordEncoder(passwordEncoder)
            return  new ProviderManager(daoAutheticationProvider)
         }

        流程
        1 UsernamePasswordAuthenticationFilter拿到用户名和密码 把它打包成一个token（Authentication类，里面有很多方法，比如isAuthtication 刚打包完是false 需要验证）
        2 token被传给前台（AuthenticationManager 已存在）
        3 token被传给公安局（Authenticationoprovider，已存在）
        4 token被传给UserDetailService（可以理解为library）
        注意
        paswordEncoder可以放在securityconfiguration外面生成 如果userdetailserveice那儿你用了userService，userservice里有用了passwordEncoder 就会产生循环依赖详细见steven 2023年2月24日视频
    9 如何在某个方法上加security？
        一般放在controller和service 推荐能在controller做就做 两个都做可能会打架。另外repository一般没人做 除非你的rep是调用第三方API 有特殊需求
        方法一：方法上加@PreAuthorize（"hasAuthority("ADMIN")"）
        方法二：@Secured（"ADMIN"）

    扩展知识
    用户访问系统的两种方式：
    1 只调用Api：带token即可
    2 有UI界面 通过SSO系统进行访问控制
    SSO：单点登录
    所有的用户认证都在这个系统进行。不然你的系统有很多个 你还需要让用户记录好几个用户名和密码吗


# 66 Docker
    干嘛的
    容器。主要用来消除：在我的环境上能运行的问题。

    为什么用
    1 多环境都可以打包成一个镜像
    2 不浪费资源（对比虚拟机来说）
    3 各个环境隔离 互不影响

    docker和虚拟机区别
    docker是操作系统之上的一个应用 占用资源少 用于将代码和依赖打包在一起 多容器共享系统内核
    虚拟机就是物理层的抽象 其中包括了一套完整的操作系统

    镜像
    程序运行的只读版本。就是个程序的snapshot 其中包含了项目启动要求的所有依赖和配置

    container
    从镜像创建的运行实例，可以运行 开始 停止 删除

    volumes
    数据 添加好后即使你container删除数据也还在


    用docker打包程序
    为什么： 因为一同连环境都打包了 不然像以前你把程序打成jar包 还得去运行的server上配环境 用docker打包生成一个镜像 你只需把镜像放在可以运行docker的地方即可

    怎么用
    gradle.builde下 弄出来个jar包
    项目下创建Dockerfile:
        # syntax=docker/dockerfile:experimental
            FROM openjdk:11 AS build
            WORKDIR /workspace/app
            
            COPY . /workspace/app
            #RUN --mount=type=cache,target=/root/.gradle ./gradlew clean build
            RUN ./gradlew clean build
            
            RUN mkdir -p build/dependency && (cd build/dependency; jar -xf ../libs/*.jar)
            
            FROM openjdk:11
            VOLUME /tmp
            ARG DEPENDENCY=/workspace/app/build/dependency
            COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
            COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
            COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
            COPY --from=build /workspace/app/scripts/start.sh /app
            RUN chmod +x /app/start.sh
            ENTRYPOINT ["sh", "-c", "/app/start.sh"]
    启动docker trminal运行：docker build -t 项目名 然后你去docker桌面的image就能看到新的image
    启动程序imagae对应的container： docker run -p 8080:8080 项目名

# 67 perfomance
    减少访问数据库次数
    小心使用findall 使用分页
    避免使用嵌套循环

# steven代码reviewfeedback
    1 不需要authority接口 因为相关信息已经被放在jwt的token信息里了。你再一个，autority这类隐私信息不允许被别人随便调去看
    2 @Transitional注解在读的时候一般不用 在以下场景可以使用： 同时修改多个表 a表修完b表没来得改程序挂了 就会出现数据不一致的情况 举例：银行系统
    3 email由于设置为unique 所以save方法有可能也会抛异常 可以try catch save一下 这样一来 单独的判断email是否存在的方法也不需要了 少查一次库
    4 把passwoedEncoder放在securityConfig外面以避免循环依赖
    5 GetMappint("/pages") pages 要删除，它不是一个资源路径 应该用？形式带参 
    6 articlePostDto 里不应该有author id，应该去jwt拿 一是因为放在dto里就意味着用户可以随意改这个值 不安全 二是因为代码冗余 应该去jwt拿
    7 article 的put和post api设计前应该加： users/{userId}/articles 这样就可以实现鉴权 不然你这个是public的 get还好 put和post不可以谁都改 一：这是设计规范 二： 满足securtiy
        这里鉴权的第二种解决方法是： 把所有put方法设计成一个接口 统一鉴权
        这里鉴权的第三种解决方法是： 公共和私人api分开放
    8 api路径不是说article开头那这个api就属于article 得看逻辑 eg： users/{userId}/articles是article的api
    9 通过id操作的可以把id放在路径 这样在securtiy过滤那儿就可以鉴定路径id和jwt id是否相同

# 68 http请求中如何判断何时该用path传参？何时该用parameter传参？
    参数含义和用途：查询参数通常用于筛选和排序数据，例如通过?sort参数按特定属性排序结果。路径参数通常用于标识资源和操作，例如通过/users/{id}路径参数获取特定用户的详细信息。因此，您应该根据参数的含义和用途来决定使用哪种类型的参数。

    参数的可选性：查询参数通常是可选的，因为您可以使用默认值或省略它们。路径参数通常是必需的，因为它们标识了特定的资源或操作。因此，您应该根据参数的可选性来决定使用哪种类型的参数。

    参数的数量：查询参数通常是多个的，因为您可以通过多个参数组合筛选和排序结果。路径参数通常是单个的，因为每个路径参数都标识了唯一的资源或操作。因此，您应该根据参数的数量来决定使用哪种类型的参数。

    综上所述，查询参数和路径参数都有其特定的用途和优点，您应该根据具体的情况和需求来决定使用哪种类型的参数。通常，如果参数用于筛选和排序数据，则使用查询参数；如果参数用于标识资源和操作，则使用路径参数。在某些情况下，您可能需要同时使用这两种类型的参数来完成特定的操作。

# 69 为什么要使用flyway？
    Flyway是一个数据库迁移工具，它可以让你管理数据库的变更。它的主要目的是确保数据库的一致性，并简化升级和迁移过程。它使用SQL脚本来更新数据库结构，并支持跨多个数据库平台，可以轻松应用在多个环境中。使用Flyway可以确保你的数据库保持最新，并且可以在线上和线下环境中快速更新。举几个使用场景：1. 在持续交付的情况下，确保系统中的数据库是最新的；2. 跨多个数据库平台统一规范数据库结构；3. 快速迁移数据库结构和数据到新的环境。总之，使用Flyway可以确保你的数据库是最新的，可以轻松应用到多个环境中，并且可以跨多个数据库平台管理数据库结构变更。分别说说它是如何做到的：
    1.它使用SQL脚本来更新数据库结构，可以在不同的数据库平台上使用，只要遵循脚本的语法规则。
    2.它使用版本控制，以确保每一次变更都是精确的，并且每一个变更都可以被还原回上一个版本。
    3.它使用校验和标记来确保每一个变更都是安全的，并且可以确保每一个SQL脚本都被正确执行。
    4.它支持自动迁移，使你可以轻松地升级或迁移数据库，而不需要手动执行SQL脚本。
    5.它支持撤销变更，可以轻松地撤销最近的一次变更，以便恢复到上一个版本。