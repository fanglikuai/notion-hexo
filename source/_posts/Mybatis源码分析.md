---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DSSBPJ4%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T190140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDCfey2JBuRZEBwjdS3vZJGSK0dWiSwD4vSgoY8neDI7wIgY2pgbABkdaf7pDQyqXEGOXA9EghTUo6GU5YcRoSBm1Aq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDPxBoY5cI9aO4wc9rSrcA9SMaeqbdAQCJKvXoTiAuSrRnwwBSv2L0O%2BmPk3xHYhUbJoZnvy%2FucfSHTLBakdWEBP%2FrAeDVyd8yTUV2DJxWcCB86e5TeDmgrc%2BRK2sjOdSbCEyHewJ12QGq9SyQVizq6Xe%2B7UF2KU1YST6I724K4brUbA33Gi5UidGuBHHiwxOCQqSPgt7ktX2NPhvSGIbYE9DBhLAGZgIEshKj023KBrKr4T8WEDAFwBddFwGa3KfKPIf1yXZLmrKuU3UBHYpIcC34i3iixDVsrsXGejOjnWm0GSployoQTd3G8XTKh2p1aVfK61eZ7esFhzyX7xtJ8SHr7vVijL5D4gCZcmMDARaXct%2B1wvSaXqoJMhaD3P2tDdkJBuu66kh9pPo2V2qL6TsINWJ886dI6vWjNI2qj7Q0DYVSDSjedpCXmNggCxEjO57uix8KlE1AiY%2ByOuIldqm3OGczt0c7xmU02n4C1990p4NcJa8VzTFM8sFsk5FPvfLoa0AlRDklT982JdpWDy7nQxQqaDmn3Nqdh4prlwvGErogfwpgwJFiB%2Ffkdxi90EWZDkQDThLPGWdvv2MwqHM0LAicwsqOAwlSxhceH4UHvROdg7OYjDdOTT%2FLs6Ji4d5b0n%2BEXZSfKKdMMuPhccGOqUBM2npAw%2Fm3Yi4FOOCkGRC0FMgRogFt4Wad%2BBBb8WGK5NSmhmBFzsSxzCtmri7KwP5BigSbd98Y%2Bn6Q4qIL2vXpTvD6C4ACDi5X8x%2FTYAub5NyDct7P8fUzC7pMHewU5BxsSQdFpje9n6lIXm%2B%2FToa8dTCgM779d40tgGo9QRBPBSw8%2BCDfwVBtEaigj%2BKR4aeaYHtKPrnfrt1JMiVvAwYP%2FK2Adav&X-Amz-Signature=882d1d27ce4c04c06cb8a3fe7c5301760322f25451f1ad07692743533868021d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {

    public static void main(String[] args) throws IOException {
        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");

        // 2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();

        // 4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);

        // 5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

        // 6.释放资源
        session.close();
        in.close();
    }
}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

