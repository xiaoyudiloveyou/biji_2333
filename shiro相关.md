# biji_2333
[Markdown基本用法](https://www.jianshu.com/p/191d1e21f7ed/)

# shiro规则
[shiro拦截器与url匹配规则](https://my.oschina.net/tij/blog/1929288)

>一、使用场景举例

         注：shiro过滤器与url匹配规则一般使用在定义的shiroFilter中，用于对指定的资源进行过滤

>二、URL匹配规则

（1）“?”：匹配一个字符，如”/admin?”，将匹配“ /admin1”、“/admin2”，但不匹配“/admin”

（2）“`*`”：匹配零个或多个字符串，如“/admin*”，将匹配“ /admin”、“/admin123”，但不匹配“/admin/1”

（3）“`**`”：匹配路径中的零个或多个路径，如“/admin/**”，将匹配“/admin/a”、“/admin/a/b”

>三、shiro过滤器

  （1）anon：匿名过滤器，表示通过了url配置的资源都可以访问，例：“/statics/**=anon”表示statics目录下所有资源都能访问

  （2）authc：基于表单的过滤器，表示通过了url配置的资源需要登录验证，否则跳转到登录，例：“/unauthor.jsp=authc”如果用户没有登录访问unauthor.jsp则直接跳转到登录

  （3）authcBasic：Basic的身份验证过滤器，表示通过了url配置的资源会提示身份验证，例：“/welcom.jsp=authcBasic”访问welcom.jsp时会弹出身份验证框

  （4）perms：权限过滤器，表示访问通过了url配置的资源会检查相应权限，例：“/statics/**=perms["user:add:*,user:modify:*"]“表示访问statics目录下的资源时只有新增和修改的权限

  （5）port：端口过滤器，表示会验证通过了url配置的资源的请求的端口号，例：“/port.jsp=port[8088]”访问port.jsp时端口号不是8088会提示错误

  （6）rest：restful类型过滤器，表示会对通过了url配置的资源进行restful风格检查，例：“/welcom=rest[user:create]”表示通过restful访问welcom资源时只有新增权限

  （7）roles：角色过滤器，表示访问通过了url配置的资源会检查是否拥有该角色，例：“/welcom.jsp=roles[admin]”表示访问welcom.jsp页面时会检查是否拥有admin角色

  （8）ssl：ssl过滤器，表示通过了url配置的资源只能通过https协议访问，例：“/welcom.jsp=ssl”表示访问welcom.jsp页面如果请求协议不是https会提示错误

  （9）user：用户过滤器，表示可以使用登录验证/记住我的方式访问通过了url配置的资源，例：“/welcom.jsp=user”表示访问welcom.jsp页面可以通过登录验证或使用记住我后访问，否则直接跳转到登录

  （10）logout：退出拦截器，表示执行logout方法后，跳转到通过了url配置的资源，例：“/logout.jsp=logout”表示执行了logout方法后直接跳转到logout.jsp页面

>四、过滤器分类

 （1）认证过滤器：anon、authcBasic、auchc、user、logout

 （2）授权过滤器：perms、roles、ssl、rest、port
 
# Thymeleaf

[Thymeleaf](https://www.cnblogs.com/msi-chen/p/10974009.html "从入门到吃灰" )


# RedisCache
>### [SpringBoot2 & RedisCacheManager](https://www.jianshu.com/p/ad168cc3603e)
>> mybtis配合redis的用法

>### [CacheManager基本配置](https://www.cnblogs.com/hujunzheng/p/10084452.html)

# springboot2.x+redis+shiro
> [redis+shiro插件的配置](https://blog.csdn.net/qq_38752386/article/details/100134270)
>> 包含的3DES加密并未使用
> 这是另一个配置方式，未验证: [redis+shiro缓存插件](https://blog.csdn.net/zzm3280/article/details/84881920)



# 其他阅读
## S:
>[实现role permission验证](https://blog.csdn.net/ruguxinyue/article/details/80587952)

>[跟我学Shiro》PDF](https://www.iteye.com/blog/jinnianshilongnian-2049092)

>[跟我学Shiro》——张开涛](http://jinnianshilongnian.iteye.com/blog/2049092)

## A:
>[Shiro使用redis作为缓存(解决shiro频繁访问Redis)(十一)
](https://blog.csdn.net/qq_34021712/article/details/80791219)
         
>[[Shiro入门]（一）使用Redis作为缓存管理器](https://blog.csdn.net/why15732625998/article/details/78729254)

>[shiro集成redis做全局session管理
](https://blog.csdn.net/kahhy/article/details/83652204)

>[问题2:shiro配置redis管理session后,每次重新请求重新生成session问题](https://blog.csdn.net/a151605/article/details/80062792)

>[Springboot+Shiro+Redis 整合](https://blog.csdn.net/qq_31897023/article/details/89082541)
## 下边是几个git：
>SpringBoot+shiro+redis
>>
1. dolyw/ShiroJwt 
2. zzycreate/spring-boot-seed 
3. saysky/SENS 
4. sunnj/story-admin 
5. chenjunwen/SpringBootFrame
6. lhrimperial/ifarm
7. 18idc/SpringBootMyBatisShiro
8. YeJR1993/SpringBoot-Mysql-Redis-RabbitMQ-Shiro
9. 511098425/springbootShiroRedis


## 这是插件的官网：
http://alexxiyang.github.io/shiro-redis/
https://www.mvnjar.com/org.crazycake/shiro-redis/3.2.3/detail.html

## B:
[spring-boot集成8：集成shiro,jwt](https://www.cnblogs.com/zhya/p/9989879.html)







