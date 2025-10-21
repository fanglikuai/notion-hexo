---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PVMD6FV%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCQY6imCtuQioLiAM8S275qQMs%2FFI4Fc4F6D3YHiQQnbAIhAI8nlO9JdtneCnI7Pr4VIJDdP6KS8sKmYpl%2FMYDL6M8BKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxivMYd7uc3uRwAYw8q3APIaAhaEbui3Loi3goUfJVprd%2FHfiIqfNzDeAlSD2Lvby7e5JOmynZGiTh97CNlB8RrCb4vE1KVSK%2BcNpE00aLbgKXhBw1TVn0i9YXo8fUtD7y6V0O2W2y%2B1icgsu%2FmAzpik6RgaJNwHd8UADVfOoJqIcQgHRMdToUP2JJOTasqesnkAdfG5r8hL1jrISkHC11agafrnLaTX5DnQR0%2FZBzPLprluQLkJAnR4oUAsjpOxXnRZH4waOQBJkdFfCFJB2JPwXDnzj4CPOaijsLkm8Cj0YQFpNHYSBmnQ4nasdPTfaJMEM7ilXeMI9C3LCDMgWEltw2RNh7EKjDuWBS%2BV78gti82lsl8aIK663ZLNbHuEOIZGyXf0bOxlt6fGn9Xw3ptiGmQFX4ULAzjpOXq8BzalqiEibih6M6vI3xMSh2jSgInIrdS8g1eCrAR7wH4b%2FtDNpcnVKbW6tQS6B9K8YHSMmXfNHhTuwgy6Cr%2BFuWvABts%2BQ0bn1D8WYH38pFkYj0pAHhPPwW07rEJrfVuX4pCxm7gQXxej7NisVrg9dZBmDKVAN5AsHL65DDEoNBofAqnyjdGMTBHz8OFa2HHg3VhAAH1N7S58Tin%2FUpeNqg49paOlW%2FQgphQgRvzwjCzq9zHBjqkAbgjKDsNkqV5JTkcH11aQXJ05QMUOx6jmbI2sFkQ5%2BLwq%2BWGW2BmzKFZlBlNpSiKYnMrtzxLiWVNJEkQwXbDjX97wEUcN6EmYX5kH1WP8gpCRk%2FZI76ZyvBmgPWSiC2JYhzqfrPTO6HhZFiFs0BNa%2B1VIcGZTh6dI%2BZ2jG7Z%2FEIGPl0xXgA5HIHjkVbiV9MCuaYCvxYePQX5qEOnGyR12dErGZfA&X-Amz-Signature=933c2d9f152d9768919b0f6d4a1e562e77ad59ab43139a56d78fb08fac80c481&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

