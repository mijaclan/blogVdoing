---
title: RESTful 学习笔记
date: 2021-6-28 23:49:46
permalink: /pages/dcebaf/
titleTag: 原创
sticky: 1
categories:
- 笔记
  tags:
- JavaScript
- 后端
- 笔记
- API
  author:
  name: mijaclan
  link: https://github.com/mijaclan
---
# RESTful 学习笔记


  参考链接：
  [极光文档](https://docs.jiguang.cn/jmessage/server/rest_api_im/)
  [js实现常用api](https://www.jianshu.com/p/1dd208488f4e)
  [nodejs实现api](https://blog.csdn.net/liyunkun888/article/details/90439108?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-4.control)

  REST全称是Representational State Transfer，中文意思是表述（编者注：通常译为表征）性状态转移。 它首次出现在2000年Roy Fielding的博士论文中，Roy Fielding是HTTP规范的主要编写者之一。 他在论文中提到："我这篇文章的写作目的，就是想在符合架构原理的前提下，理解和评估以网络为基础的应用软件的架构设计，得到一个功能强、性能好、适宜通信的架构。REST指的是一组架构约束条件和原则。" 如果一个架构符合REST的约束条件和原则，我们就称它为RESTful架构。

  REST本身并没有创造新的技术、组件或服务，而隐藏在RESTful背后的理念就是使用Web的现有特征和能力， 更好地使用现有Web标准中的一些准则和约束。虽然REST本身受Web技术的影响很深， 但是理论上REST架构风格并不是绑定在HTTP上，只不过目前HTTP是唯一与REST相关的实例。 所以我们这里描述的REST也是通过HTTP实现的REST。


### 统一资源接口
RESTful架构应该遵循统一接口原则，统一接口包含了一组受限的预定义的操作，不论什么样的资源，都是通过使用相同的接口进行资源的访问。接口应该使用标准的HTTP方法如GET，PUT和POST，并遵循这些方法的语义。

如果按照HTTP方法的语义来暴露资源，那么接口将会拥有安全性和幂等性的特性，例如GET和HEAD请求都是安全的， 无论请求多少次，都不会改变服务器状态。而GET、HEAD、PUT和DELETE请求都是幂等的，无论对资源操作多少次， 结果总是一样的，后面的请求并不会产生比第一次更多的影响。

下面列出了GET，DELETE，PUT和POST的典型用法:

### GET
  安全且幂等
  获取表示
  变更时获取表示（缓存）
  200（OK） - 表示已在响应中发出
  204（无内容） - 资源有空表示
  301（Moved Permanently） - 资源的URI已被更新
  303（See Other） - 其他（如，负载均衡）
  304（not modified）- 资源未更改（缓存）
  400 （bad request）- 指代坏请求（如，参数错误）
  404 （not found）- 资源不存在
  406 （not acceptable）- 服务端不支持所需表示
  500 （internal server error）- 通用错误响应
  503 （Service Unavailable）- 服务端当前无法处理请求
  POST
  不安全且不幂等
  使用服务端管理的（自动产生）的实例号创建资源
  创建子资源
  部分更新资源
  如果没有被修改，则不过更新资源（乐观锁）
  200（OK）- 如果现有资源已被更改
  201（created）- 如果新资源被创建
  202（accepted）- 已接受处理请求但尚未完成（异步处理）
  301（Moved Permanently）- 资源的URI被更新
  303（See Other）- 其他（如，负载均衡）
  400（bad request）- 指代坏请求
  404 （not found）- 资源不存在
  406 （not acceptable）- 服务端不支持所需表示
  409 （conflict）- 通用冲突
  412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
  415 （unsupported media type）- 接受到的表示不受支持
  500 （internal server error）- 通用错误响应
  503 （Service Unavailable）- 服务当前无法处理请求
### PUT
  不安全但幂等
  用客户端管理的实例号创建一个资源
  通过替换的方式更新资源
  如果未被修改，则更新资源（乐观锁）
  200 （OK）- 如果已存在资源被更改
  201 （created）- 如果新资源被创建
  301（Moved Permanently）- 资源的URI已更改
  303 （See Other）- 其他（如，负载均衡）
  400 （bad request）- 指代坏请求
  404 （not found）- 资源不存在
  406 （not acceptable）- 服务端不支持所需表示
  409 （conflict）- 通用冲突
  412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
  415 （unsupported media type）- 接受到的表示不受支持
  500 （internal server error）- 通用错误响应
  503 （Service Unavailable）- 服务当前无法处理请求
### DELETE
  不安全但幂等
  删除资源
  200 （OK）- 资源已被删除
  301 （Moved Permanently）- 资源的URI已更改
  303 （See Other）- 其他，如负载均衡
  400 （bad request）- 指代坏请求
  404 （not found）- 资源不存在
  409 （conflict）- 通用冲突
  500 （internal server error）- 通用错误响应
  503 （Service Unavailable）- 服务端当前无法处理请求

### 理解RESTful
要理解RESTful架构，需要理解Representational State Transfer这个词组到底是什么意思，它的每一个词都有些什么涵义。

下面我们结合REST原则，围绕资源展开讨论，从资源的定义、获取、表述、关联、状态变迁等角度，列举一些关键概念并加以解释。

资源与URI
统一资源接口
资源的表述
资源的链接
状态的转移

1.主流的是http1.1协议
2.先有REST理论，再设计http1.1协议

### REST
#### Resource
所谓资源",就是网络上的一个实体,或者说是网络上的一个具体信息。它可以是一段文本、一张图片、一首歌曲、一种服务,总之就是一个具体的实在。你可以用一个URl(统一资源定位符)指向它,每种資源对应一个特定的URI。要获取这个资源,访问它的URI就可以,因此URl就成了每一个资源的地址或独一无二的识别符。
http:/www.wolfcode.cn/news
总结:
1在网上一切皆资源
2每一种资源都有一个特定的url指向它

#### Representation 资源的表现层，资源的表现形式
资源是一种信息实体,它可以有多种外在表现形式。我们把资源具体呈现出来的形式,叫做它的”表现层”( Representation)   具体呈现出来的形式

比如,文本可以用txt格式表现,也可以用HTML格式、XML格式、JSON格式表现,甚至可以采用二进制格式;图片可以用JPG格式表现,也可以用PNG格式表现。

URL只代表资源的实体,不代表它的形式。它的具体表现形式,应该在http请求的头信息中用Accept和 Content-Type殷指店,这两个字段才是对表现层”的述。
accept:application/json 浏览器->后台
content-type: application/json响应格式

总结:
1. 资源以什么样的表现形式呈现出来这个就叫它的表现层
2. 资源的表现是一般都是在请求头信息中体现出来

#### State Transfer  状态转化

访问一个网站,就代表了客户端和服务器的一个互动过程。在这个过程中,势必涉及到数据和状态的变化。

互联网通信协议HTTP协议,是一个无状态协议。这意味着,所有的状态都保存在服务器端。因此,如果客户端想要操作服务器,必须通过某种手段,让服务器端发生"状态转化"(StateTransfer)。
而这种转化是建立在表现层之上的,所以就是表现层状态转化”。

改变(服务器端)资源的状态

改变(服务器端)资源的状态
http://www.wolfcode.cn/employees
新增:从无到有状态的变化
更新:从某个状态变成另外一种状态的转化
删除:从有到无状态的变化

总结:
1. 在客户端和服努器的交互过程中涉及到状态的转化我们是通过某种手段 ajax, form)让它进行转化

#### Uniform Interface 统一接口
统一接口
REST要求,必须通过统一的接囗来对资源执行各种操作。对于每个资源只能执行一组有限的操作。

HTTP11协议为例
7个HTTP方法:GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS
HTTP头信息(可自走义)
HTTP啊应状态代码(可自走义)
这些就是HTTP1.1协议提供的统一接口

REST还要求,对于资源执行的操作,其操作语义必须由HTTP消息体之前完全表达,不能将操作语义封装在HTTP消息体内部。

http:/www.wolfcode.cn/employees
新增:从无到有状态的变
更新:从某个状态变成另外一种状态的转化
删除:从有到无状态的变化

以前
http://www.wolfcode.cn/employee/saveorupdate.do?id=1&name=xx
现在
http://www.wolfcode.cn/emplovees

总结:
1.我们会使用7个HTTP方法来体现我们现在对资源的操作不再在url地址上体现

#### 关于应用接口
系统的接口:很多情况下需要把系统的功能作为服务暴露绘外部的其他应用使用或者给移动端使用就需要把系统中的服务作为接囗暴露出去一股分为公共接口(发短信,天气服务)和私用接口(公司内部使用的)

公用接口：
示例: 京东万象


#### RESTful设计
1. 资源设计
   * 路径
      使用以前的方式完成需求
      1帖子列表支持过滤和分页;   htt://bbs.wolfcode.cn/bbs list. do? keyword=x& currentPage=1   =>list
      2,查看一篇帖子;           http://bbs.wolfcode.cn/bbsview.do?id=xx                         =>view
      3查看一筒帖子的所有回帖;   htp://bbs.wolfcode.cn/bbs replay list. do?id=x                 =>replay
      4.回帖;                   http://bbs.wolfcode.cn/bbsreplay.do?id=xx&content=xxx
      5发布一筒帖子;            http://bbs.wolfcode.cn/bbspost.do?title=xx&content=xxx          =>post
      6修改一篇帖子;            http://bbs.wolfcode.cn/bbsupdate.do?title=xx&content=xxx&id=xx   =>update
      7,删除一篇帖子;           http://bbs.wolfcode.cn/bbsdelete.do?id=xx                       =>delete
      
      这种方式的问题大量的接口方法URL地址设计复杂需要在URL里面表示出资源及其操作;
      
      路径叉称终点"( endpoint),表示AP的具体网址。
      在 RESTful架构中,每个网址代表一种资源( resource),所以网址中不能有动词,只能有名词,而且所用的名词往往与数据库的表格名对应。一般来说,数据库中的表都是同种记录的"集合"( collection),所以UR中的名词也应该使用复数。
      
      名词也应该使用复数。
      https://api.example.com/v1/zoos:动物园资源
      tps://api.example.com/1/animals:动物资
      https://lapi.examplecom/v1/employees:饲养员资源
      参考例子
      https://api.github.com/
      http://docsiquang.cn/imessage/server/restapiiml
      总结:
      1.设计资源的接口,uri选用的是名词一股和数据库表名相同,并且使用复数

      抽象出来的接口:images图片资源的接囗post上传，delete删除

1. 动作设计
   * HTTP动作
      GET(SELECT)：从服务器取出资源(一项或多项
      POST( CREATE):在服务器新建一个资源。
      PUT(UPDATE):在服务器更新资源(客户端提供改变后的完整资源)。PUT更新整个对象
      PATCH( UPDATE):在服务器更新资源(客户端提供改变的属性【补丁】)。 PATCH更新个别属性
      DELETE( DELETE):从服务器删除资源
      
      HEAD:获得一个资源的元数据,比如竞源的hash值或煮最后修改目期
   
      OPTIONS:获得客户端针对一个资源能够实施的操作;(取该资源的api能够对资源做什么操作的插述
   
   
   * 动作示例
      GET/zoos:列出所有动物园
      POST/zoos:新建一个动物园
      GET/zoos/ID:获取某个指定动物园的信息
      PUT/zoos/ID:更新某个指定动物园的信息(提供该动物园的全部信息)
      PATCH/zoos/ID:更新某个指定动物园的信息(提供该动物园的部分信息)
      DELETE/zoos/ID:删除某个动物园
      GET/zoos/ID/animals:列出某个指定动物园的所有动物
      
      获取某个部门的所有员工
      GET /deparments/ID/employees


2. 返回结果
   * 返回值类型
      GET/zoos:返回资源对象的列表(数组/集合)
      GET/zoos/1:返回单个资源对象
      POST/ collection:返回新生成的资源对象
      PUT/ collection/resource:返回完整的资源对象
      PATCH/ collection/resource:返回完整的资源对象
      DELETE/ collection/resource:返回一个空文档

   * 常见状态码
      200 oK-[GET]:服务器成功返回用户请求的数据。
      201 CREATED-[POST/PUT/ PATCH]:用户新建或修改数据成功
      202 Accepted-[*]:表示一个请求已经进入后台排队(异步任务)
      204 NO CONTENT-[*] DELETE]:用户删除数据成功
      400 INVALID REQUEST·[POsT/PUT/ PATCH]:用户发出的请求有错误,服务器没有进行新建或修改数据的操作,该操作是君等的。
      401 Unauthorized-[*]:表示用户没有权限(令牌、用户名、密码错误)
      403 Forbidden-[*]表示用户得到授权(与401错误相对),但是访问是被禁止的
      404 NOT FOUND·[*]:用户发出的请求针对的是不存在的记录,服务器没有进行操作,该操作是幂等的
      406 Not Acceptable[GET]:用户请求的格式不可得(比如用户请求丿SON楂式,但是只有XML格式)。
      410G。ne-GET:用户请求的资源被永久删除,且不会再得到的
      processable entity[PosT/ PUT/PATCH]当创建一个对象时,发生一个验证错误。
      500 INTERNAL SERVER ERROR·[*]:服务器发生错误,用户将无法判断发出的请求是否成功。
   
   * Content Type 
      1,一个API可以允许返回json,xml基柔html等京撩式;建议使用json
      2,以前通过URL来规定获取得格式类型,比如
      https://api.example.com/employeeison
      https://api.example.com/employee.htmi等
      
      但是更建议使用AccePt这个请求头;
      Accept：客户端希望出现的数据类型
      Accept与Content- Type的区别
      1.Accept属于请求头, Content-Type属于实体头。
      Http据头分为通用报头,请求报头,响应报头和实体报头
      请求方的http报头绩构:通用报头| 请求报头 |实体报头
      响应方的http报头结枃:通用报头| 应报头 |实体报头
      2。Accept代表发送端(客户端)希望接受的数据类型
      比如: Accept: application/json
      代表务户荣养翼持凳的教据型是9n理原返回5n教据
      
      Content-Type代表发送端(客户/端务器)发送的实体数据的数据类型。
      比如: Content-Type: application/json
      代表发送端发送的数据格式是json,后台就要以这种格式来接收前端发过来的数据。
      
      二者合起来
      Accept: application/json
      Content-Type:application/json
      即代表希累接票的教据装型是json式,本次请求常送的教据的教据式也json式


#### RESTful服务开发
##### java中常见的RESTful开发框架
1. jersey
  jersey RESTful架是开源的 RESTful相架实现了JAX-RS(JSR 311& JSR 339)规范。它扩展了 JAX-RS参考实现,提供了更多的特性和工具,可以进一步地简化 RESTful service和 client开发。尽管相对年轻,它已经是一个产品级的 RESTful
  service和 client框架

  优点：
  优秀的文档和例子
  平滑的JUnit集成
  就个人而言,当开发 RESTful service时, JAX-RS(傅使用RESTful风格来开发 web service服务的规范)实现要好于MvC框架
  可以集成到其它库/框架( Grizzly,Netty这也可能是很多产品使用它的原因
  不喜欢 servlet container?傅用jersey的时候可以不用它们
  WADL, XML/JSON support
  
  缺点:
  Jersey2.0+使用了有些复杂的依赖注入实现
  大堆第三方库只支持 Jersey 1.x  在 Jersey2.X不可用


2. play

3. SpringMVC

##### 使用SPringMVC开发RESTful服务
需求:
1.获取所有的员工(集合)

  设计 restful接囗步骤
  1).确定资源/ employees
  2).确定请求方式GET
  3).确定返回结果类型头信息状态码)员工集合, content-type=application/json,200

2.获取某个员工的信息(路径传参)

  设计 restful接囗步骤
  1).确定资源 /employees/{name}/{id} 使用路径占位符
  2).确定请求方式GET
  3).确定返回结果类型头信息状态码)员工集合, content-type=application/json,200

  如果@PathVariable注解没有设置value,默认就是去路径上找相同名称的参数

3.删除一个员工
  1).确定资源 /employees/{id} 使用路径占位符
  2).确定请求方式 DELETE
  3).确定返回结果(类型,头信息,状态码)员工集合,空文档,204


4.获取某个员工某个月的薪资记录

  1).确定资源 /employees/{employeeId}/salaries/{month} 使用路径占位符
  2).确定请求方式 GET
  3).确定返回结果类型头信息状态码) 薪资对象, content-type=application/json,200

  @DateTimeFormat 前台传日期参数到后台接收是使用的注解
  @JsonFormat 后台返回json数据给前台时时有的注解

5.给某个员工添加一条薪资记录
  表单提交再做
  1).确定资源 /employees/{employeeId}/salaries
  2).确定请求方式 POST
  3).确定返回结果类型头信息状态码) 薪资对象, content-type=application/json,200

  路径占位符中的参数，可以自动封装到自定义对象中的同名属性上

6.更新员工数据

1. API接口测试
   * postman
  
2. @RequestMapping
   * RequestMapping的参数化
  
   * @PathVariable
   
   * RequestMapping的继承
   
   * @RequestMapping的属性
    params是规定请求是必须指定的参数名称和参数值
      代码例子：代表请求是，必须带有name参数，并且值也必须是admin
      @RequestParam注解，规定请求时必须带有指定的参数，但是参数数值不规定
    
    headers规定，请求时必须带有指定的头信息
    header="content-type=text/html"
    header="content-type=text/xml"

    消费相当于配置了header="content-type=text/xml"

    生产的数据类型
    相当于配置了header="accept=text/xml"
    还代表响应头信息中有content-type=text/xml

   * @RequestBody
    把请求体的所有内容都封装到指定的对象中去

3. 请求处理
   * Ajax
    使Ax发送条种清求京法的请求：
    $.ajax({
      url:"/job/id",
      type: "DELETE",
      dataType:"json",
      success:function(){

      }
    })
    
    处理put或 patch请求方式的过滤器


   * from
    可以使用过器对请求方法进行修改:
    1.在请求中使用 method= DELETE来提交请求方法
    2.使用springMVC提供的org.springframework.web.filter.HiddenHttpMethodFilter转化
    浏览器不交持put,delete等method,由该filter将/blog?_method=delete转换为标准的http delete方法


小结：

1. 简单理解rest相关的概念
2. 知道私用接国和公共接区别
3. RESTfu设计
  3.1 必须掌握资源设计
  3.2 必须拿握请求动作
  3.3 返回结果了解一下
4. 得知道有哪些 RESTful开发框架
5. postman测试工具常用的功能必须掌握
6. springMVC中跟rest相关的属性和注解都需要拿握(更多在项目中练习)
7. 掌揠ajax和form请求提交put方法的操作











