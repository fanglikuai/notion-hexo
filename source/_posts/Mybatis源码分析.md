---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSDYL5YU%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB3jWRZKpTq3lt1bmluElpUN02wIdnsvjdrR2AAibT0tAiEA3VPMeZ8nVeq%2BGMZnWW88PwLJGdN2stRfiyugIa1BSEwqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDDTJXQ7kc6jpbx%2FyyrcA0jdq7cpu1WCas1c6bx%2B%2FYqSqGQxEHPRHIGsuj3hEhoIrJ%2BIUmWqT87Ab3oYrKfDdIyscYDCP2yIacbXvzVgmnVx7HNGD8YP2bHHDzNLu%2B3cIxHcRigDO29wjUOrjH88tGfDq6Mg4ywedAzM8nBkqgHaquHJJNEjhoU6ATp9nVvhjHSghg2EKfNieA56VGPmcJ%2B0KJOh4Lksypohb2Y%2FK%2Fk%2FQDdlNKzFeUrob6jpwOiGDL%2B9v5nd%2FPCpSas0%2FZGp%2FWDEbQHthiUHsTgHitfK6YFowtRKI7IzfXeN5u2Bs4TXTlrLVc3nT13HtwRREuuo7ooPHJM4rkNujK3UF%2BUygwF8MUVxCp0UYanuLCqXTNeEn0Ws%2FlyVuTEe0W3G3qmjBTZHkUOvT1JAu95N6ByOxSvsT4WfaI22Syc%2FyNf8Ruw46kj%2BnApzcDunYeV680QChtKuOL2B73EINBJTDVi3DgexkeHHlVOqa0VKKxK1cpBV1MJo5IfNtk0T%2F4T1c6XCFJ6nrTODsjKlTKbOqn9xk2yS8xGI4EpjjAzFtxljYvPlXnO1u7yvBgOAQprtb9o%2B%2FET%2FScpr6jZ%2FVorRx3ufq0gN6x3JgMNzcGMhTD1rKh9jjlocO3U4KQyQMIQiMKTdtcgGOqUBqnbKPzTIUNtz0Fo56aWpUVAZkBlfsdKHKvo3iA%2BC1bOjzWfRx2Xx5fBthLc2ZSwjLBNN6eRb13zZbhA7ho0ee%2FeyUPcVe860fu4MPtSxEt%2Fq6IYN5zaeFzNNFv87SVaZYcPvwhzyBBqxjTOcFRUFb%2BCclznbYFb%2BNuI27TWHoNWr5JuMPr%2FQOBzVVMSf2NP6l%2BHp5O77uGuGnfhVSx1xJFZqPdMt&X-Amz-Signature=9a2431e5a71944edb3d5d83f82017a6cbb2eeba329e84f733cee15f2df23f7f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

