---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ5XBW6N%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T230126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGHnfpSK%2F7gLX1futJUkcA7DL%2BRW2cGFi0wx1Cq1tYqWAiBrJqG%2BeQXjwIUhX8S8QZ58Qrz8PSdEUHT9IutYOVuVfSqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIo2UCPKHrxTR3or4KtwDl7jkvMDrJL05Qdk9VfoHeMd8bnVFREyyPW%2Fj5EWAP%2FpaxZPv5aIL4QWb%2Bo%2Fn%2BmLBAIM15u4fUaSsq1k%2BW6paTeBWVSPr9Evly%2BXOSN0vBW6pKpN053DaGLbS7q5vq0QNfHueo8pfjRDQ1yAr3ZyTr%2Ba2jGux5jev6V1QRbf2QSFZmu1xaptBgf5fZ7YY6vA53%2FSAZ2lbyc%2Fz%2BGpomGVStv%2F0uxgCcO19S0UUHHMyjdEcMHIlhwxHKGl1TT4SIInjgdfs%2BGKgECVzoJNqtcB0nnJlamU%2Bpg14iRVUWi0ZBbTLUGDH6DOY%2BLFdVcyBKpmSQxZL5UacE9MxmOM95VuEutEvkirxd8kunheLdawDmJ9SSJJ8htH59z5n1wv4XvOGXT2380Dz9dT5jgzQRajW50wSNve8%2BnAghN2TtJCuVTlFXyJ79bDK8UQEr0Pvc3rWrmhnIbE2wnyxOlYeLjTExkp%2F2Ww9nRyOq1Jquxqc2fFKq1orT4SM9HGNUQ5WHbF0P4DqBPRRtVx6EDtl7YSpUXRAdKiYv%2F633aiXsAjoSj0S5QnwCYq4IizP0RjQidDHaRUE7AQz9lGnuGIzoeU0nEgiuxzGmRfCXHn7Lbl%2F5NbUCsOBVtPHH%2BOk87YwmNjFxwY6pgHk8uVATZtBeB0WY16TMQJk%2BZxG5dEGh3j2bDwk2hJXPcoR6AWjRfGsSxWrAP6BnqvSu6RupUlayal1OZf%2B1uXBBBZMmVtzs7%2FZ2ngktA9IRv19LdZEKwtPEEy7vjL5jpyANAX7ok%2F74ZjDcIlWSyxwOZgFFkbeJEZ2ktff6Jzx89AS5y4s4VOwsH1vBOfq0%2BgdzloZqVzuJ2P5qxjNsTc1%2BO44srHK&X-Amz-Signature=f71ff6397cc6c57fe6009cb46891718cd885714d5a6facbd31aa484a2a31ecc5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

