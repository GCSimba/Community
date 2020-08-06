# Community

一、首页搭建模块：
1.	数据库建表：user表、comment表、discuss_post表、login_ticket表、user表
2.	创建User、discussPost、Page模型。
3.	DiscussPostDAO:分页查询帖子的数量、查询帖子的行数(方便分页)
  DiscussPost-mapper.xml配置DiscussPostDAO映射
  UserDao：根据id查user、根据username查user、根据email查询user、adduser、updateStatus、UpdateHeaderUrl、UpdatePassword（注解映射，无需配置mapper）
4.	DiscussPostService：getDiscussPosts、findDiscussPostRows
  UserService：根据id查用户
5.	HomeController:getIndexPage():展示首页的内容

二、登陆模块
三、核心模块
四、高性能存储方案
五、Kafka异步消息队列
