---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVPLTD6K%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQC2ASy%2F%2BWIxgsBF3xIiXKL2aduNQeWkB3pdXdESKka4ngIhAJM7BdhfJhqX5WsD90Z%2BGS2B5fJ%2Fe6me0WpLFr6aQa4CKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwa9Yck0piaLHHpXPIq3AMWISM4OgOvQ%2B%2BozwmPKjbUAMKB2tUHjD%2BVqHwU4XZzoz3NWNt0oBZjB8iH6yudFzUyLpZS%2BlLNC3mMUlVa9NMz5b7e2yCbDzFaAoFgPPERlHxJfjHUDH6%2BhusNGHJbUaDEPvQ%2FlQN1a1xK%2Fn%2Fid8COgP5CFcBNTSp2zGEXboCSYMi%2FxpE1ySyNu81LDykThm%2FBm2pCcwMTQ4sXn1jvoxJ8Accsqg2s0mcDK5lXYJWeqstUDZr8w%2BgQmDtBysQrjAf%2BcPV2G9MRieuamq30QVz2Vy0Crvs%2BX9AlgXqp5PRH1uL3sF5lkjSZpaftDJqDrYBI9kMwCxUCvdRVTl0D1hPTfClqzxYjH6Q7sV5Xfsvksz81Uq0VEQoG0H1anCHKGx6lriwPUOirXp4gMAgJlVB8DkyJ%2Fe4WGLB0JMNpAOzecKqnAvJrrdE8sQUWJTIP4zQsbQee2YwVOkO8MD%2FuzEVMhKzY9iGLy3Z3fdZkeB824jK7VApBXunzqcK4D35HWd2pfXX7I%2FHEG9OjM3M4FW4twT2LmHqCLjAsIsVxCDNX7cFGRJAbNu7U1wntsI5oMH6u317oIsyTXsymvvdcpiL5DdndwgDXd2H0OSwdUa81sV4%2Bm2%2BPCEUxGILTTzDwqeHGBjqkAYt4IbyHPiYmbRegYjiTgJqPrKWmJGa05K%2Bj5n0%2BYUJxEguFp0qQmC%2FT9B%2B8JQQaoRyQLUgpnm5Wpl%2BQxNI6aMihlCB7NXduA%2Fb8nqdMAd9YCcZj8Te1oafRRwCFUmaHQS5NeLrHSN8TpXBzxIkWGP93ZyXFD%2FwpgG9q8P%2BYDTPhaxNIFIBfhmnQ6JD8QJSXQNgAptuYya3JpjeFhwHg%2BkonaT2v&X-Amz-Signature=fdc630194e25d1c8f0cfc20443dbef43c83a5a5d012f923dd1475fb94a1376e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

