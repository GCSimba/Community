# Community

#### 一、首页搭建：

**1.** 数据库建表：user表、comment表、discuss_post表、login_ticket表、user表

**2.** 创建User、discussPost、Page模型。

**3.** DiscussPostDAO: 分页查询帖子的数量、查询帖子的行数(方便分页) DiscussPost-mapper.xml 配置DiscussPostDAO映射

 UserDao：根据id查user、根据username查user、根据email查询user、adduser、updateStatus、UpdateHeaderUrl、UpdatePassword（注解映射，无需配置mapper）

**4.** DiscussPostService：getDiscussPosts、findDiscussPostRows

 UserService：根据id查用户

**5.** HomeController:getIndexPage():  展示首页的内容

------

#### 二、登陆模块

##### 1.发送邮件

邮箱设置 -启动客户端SMTP服务，配置邮箱参数，使用javaMailSender发送邮件

配置邮件客户端：配置application.properties、MailClient类

##### 2.注册功能

UserDao：addUser

创建CommunityUtil类：生成随机字符串；MD5加密

UserService：注册Register

LoginController：显示注册页面、实现注册功能

##### 3.邮件激活功能

创建CommunityConstant常量工具类存储静态常量

UserService：激活邮件的功能

UserController：获取登录页面的方法、激活邮件的方法

##### 4.获取验证码

创建验证码配置类：KaptchaConfig

LoginController：生成验证码的方法(/kaptcha)

##### 5.登录功能

创建LoginTicket模型

LoginTicketDAO：insertLoginTicket() 、 selectByTicket 、updateStatus

UserService:login()、logout

LoginController：login登录方法

##### 6.显示登陆信息

创建HostHolder和CookieUtil工具类

创建LoginTicketInterceptor拦截器拦截用户请求

创建WebMVCConfig类注册拦截器

##### 7.上传头像

UserService：上传头像的功能

UserController：uploadHeader()上传头像、getHeader()获取头像 

##### 8.登录校验

创建LoginRequired注解

创建LoginRequiredInterceptor拦截器拦截请求

在WebMVCConfig类注册拦截器

UserController：为需要进行登录校验的方法添上LoginRequired注解

------

#### 三、核心模块

##### 1.敏感词过滤

利用字典树编写敏感词过滤的类

创建一个用于存放敏感词的text文本文件

##### 2.发布帖子

在CommunityUtil中获取Json格式字符串的功能

DiscussPostDao：addDiscussPost()

DiscussPostService:addDiscussPost()，需要进行敏感词过滤和HTML标签过滤

DiscussPostController：@postMapping(“/add”) : addDiscussPost()

##### 3.查看帖子详情

DiscussPostDao：selectDiscussPostById()

DiscussPostService:findDiscussPostById()

DiscussPostController:getDiscussPost()

##### 4.显示评论

在CommunityConstant配置实体类型：帖子和评论 

创建Comment模型

CommentDao:根据实体查询所有的评论分页展示 、查询评论的数量

Comment-mapper.xml映射

CommentService：根据实体查询所有的评论分页展示 、查询评论的数量

DiscussController：getDiscussPost()

CommentDao：新增评论，更新评论的数量

DiscussPostDao：更新帖子的数量

CommentService：查询评论的数量，更新评论的数量  

DiscussPostService:更新帖子的数量

CommentController：增加评论

##### 5.私信列表

sql设计：message:

id	from_id	to_id	conversation_id	content	status	create_time

<!--这里conversation_id中的两个人的id拼接会出现111_112=112_111但其实两个id是同一个对话，所以这里统一规定序号小的放在前面-->

MessageDao：查询当前用户的会话列表，（分页） selectConversations()

​    查询当前用户的会话数量 selectConversationCount()

​    查询某个会话所包含的私信列表（分页） selectLetters()

​    查询某个对话所包含的私信数量 selectLetterCount()

​    查询未读私信的数量 selectLetterUnreadCount() 

​    根据MessageDao写Message-mapper.xml配置文件

MessageService：实现MessageDao的相关方法

MessageController（处理私信列表的请求）

私信列表：getLetterList()

​    私信详情：getLetterDetail()

​    辅助方法：getLetterTarget得到私信来自于哪个用户

##### 6.发送私信

messageDao：新增私信，修改多个私信的状态(已读or删除)

messageService：新增私信（addmessage）：敏感词过滤，HTML过滤

​          读取消息，将消息的状态置为1

MessageController：辅助方法：或许发送私信的目标用户

​           在私信列表中将私信设置为已读

​           SendMessage()，返回Json字符串格式：0表示发送成功 

##### 7.统一处理异常：

homeController：获取500.error页面

通知方法：ExceptionAdvice

##### 8.统一记录日志

编写一个切面：ServiceLogAspect（AOP）

------



四、高性能存储方案
五、Kafka
做消息队列



六、分布式搜索引擎
elastic search
全文搜索
