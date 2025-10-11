---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIM54VPV%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDL7q4QgCoZdZk2YzoRXuHlIsQwHqceZWO8BWGtx73hfwIhAL8rrYJljpEfckrSKIk34ft7E44PDXRsh1Lq5dmEqOcaKv8DCBYQABoMNjM3NDIzMTgzODA1IgyhFmwBWtNReIUzDJsq3AMZFeBwIQ9FRXtllUTd1pmuAE1BrqgwIN7O61vVomYTreEZFjRZV29M7HpeM6PBYvTi9Fs%2BWj3LMWJyCnYe1x3KKbo9mCbepTid2SXfUI1EVA6gSOe1nvGkQXSU9dHLZnV8ewezRw8U4CnRa%2FvuVvfnqO7lrtqRYenzh8y2sk76s92XstMxHdgzt78xhlun%2FbX5A25J7ZWrjORpCRAdIkegUMTtytQz7P8QwGQj0ee1pQG3ODnDLlNFWZpfu1FG2XqLX9Ea9xxHbcpRJBySzL%2Fu%2FU1fwJuoe%2FhEyL7GCbqaOp4alCzAHaYP5klREr5hapK%2FQ%2FAw3SlczOeOup53kGKtM3mH03uPr05sfMdMfO733miCjzL%2F%2Br05e3SeEOG6ClTO1KQVot5rWM2unCZCJ%2F6mrDGolwC4qKSihpAV2Lk8IK%2FQkFyN%2BDnhvbJG3zZ7%2B7tRJArqnuXXitQywY3xP%2BUJ7c2mYk494RmyemOh3y7aTto4JSu6G%2BSVE02Vhw5mKWF9I%2FF4PcRYNyqtn42bK9ov6uPpnouDbPO7jqzXcjmenrv6fBGGoSqGDI0h2XPA1wolLWyLh4Ei2foC6yU4pF%2B2E3yla0zx8MrM34OUIEzYXlhkpPLcs7y9%2FdFz8jCVpKnHBjqkAYrOsewUBGz%2BSX5LPe3BlUs1VIkndCAPnQpVhF7BLhqxeGeFhPNHXyMXS5DXP9LIevzE%2BeOEADBfrdzuLo2CEIgNCarrzk5Oikut%2FwxV0aq4GvNm5SsrtyiEXeobfRvI%2BpI0ER8O0d5Okb8vj4%2FeGnabhVhgXo8%2BMe0io7uxBdfkFzCEANmnlxLlLEFunptttcmFhhJZ9ID5KNHJud31VF1mfaEu&X-Amz-Signature=5f3d4f481f7fd1e7b6f034fd3a532142c120eea23674f2a029d10225719c8253&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

