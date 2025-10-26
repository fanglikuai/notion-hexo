---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVU43BX%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU9M%2Fyt9wJ%2BkRGyD4wphAIICg8Rh0XTyjPzGjpu1YlPAiEAsP%2B9Cece%2FXcqWQQ4S7ycCu63GIA0C%2FoqPIBBkGYbSn0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEWiH6lg5FjMpGpfSrcAzuDhYCTQIf9kxeespYdk4gM%2BOqVoaSrSNb6RVn8p1kWz81VZ2YaDkv2lDLWDLjjfBRc93t%2FvjFIi%2B%2BMtOEVewazfJ%2BFUudRZ6Z56TQfmJzOKWdswomJrYsdcBMwN3wkT50IPqV%2FX5asdQhtobbdNG8aqO0JXbqRw41AbQWcntuxTJcQXf5dKrM270mH4piyqafhIVd6Gex7PzwSy8Zqt8QVCu2HUTgh750Rd9ucKzj9%2BkFwGtoi%2BFr5fwU7schoTsqtI2vGgI6h6KqAi5lCrJibtrr6zcJ9CmYSi%2Bjomf8X2xlA5%2Fo6hUGFgpg4%2Bpqu6MjRMdUGorjGK%2BK45u6D6gvC%2FTmFeueHVnKmppwDHzXtI4SQSdEUGxg6cr%2FEdtjUszjyW0P5bQxrKBtorkuTpq%2Fz0AwkKfBbE%2B8MoqDmF6DuQU8LO8zXAYEoA9QkV31At1tTOwo0wnnqQxXXT0zOhb8PunxXk9816PS7w6cPaoUHWD5Pf3bzamxdrTBqSWgVtDAy1zEjV6iORZc2PQpRYZRDIUhmkucno7RhY0serzn4wbb5akjLeiDf9c4qZQzbWgcqtLnk6PjKf09sdDQeZynvTDWwr6Xj9M4z8HR1AT%2FL0SkBMq0mmhCpDvAkMKLw9ccGOqUBdY%2FLjSFPoRuqObo0N7M9KgeMfi1uBpmB1sm0vshJ32zXaFrOKhZXQ10IWuTh1SXhiL9pU1r%2FJPB7WTrIBEblKz90EU4jk%2BufEwIcKPz3GZuzpgzT4T1ksBwL0se9twwAAc4irucltiqwYdYVAHCCEOtLix8dpp7Hz0lhfUXbMGe19n5AsFG%2F3a%2BZzrf4wSYYPu3Y05DmkXoFeENnVaS7Z%2FINYA3y&X-Amz-Signature=34cadc9c9677b9cc28c4b3887ce79616c57becf9228b61bd99cb96a240fccfc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

