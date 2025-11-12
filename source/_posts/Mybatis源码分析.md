---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJZYPGY6%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQCZrJ0fWNr72%2BUN1Ux3fXqC%2Bj1ibgZ6YNIcwuPuuraK9gIhANGG04RbPkwfLy6%2BNFVlKJwp53plpTyYoVJeMXd8pEdtKv8DCDgQABoMNjM3NDIzMTgzODA1IgxYw79WaUD1Vo7gQEMq3AObOHp1GK9yKQMdRiRa8qiFJlSi0dNZDX197xtCeZKa1eBBsjQiQ5BxaFT7Owu6j%2FFNbvjT7Aer0o2A9A9VTAzy6zZv7sojVw2GzA7enRDZSlCBpSis19Y0AHqXGZe5uzqTM2xsnaW4%2FeIY%2BEdqiU2AL9hFMIfwp3VqNh108v80Qf6hAqkJJOhF%2FoKK0BRg6rAcuAtWyZmS46g4r93kYQ5AOcxp%2BRSKB7VZFpaxCLMYkvxQLlYji2Rtb0UErW9BgGRpnWK2f5UtkwvcyFr1QiZPQlqtY%2F2iNLxkILdHOcP83%2BrI%2BtrNWzum5CDVoAW1sELBg3m8On8g9OruwYwGI6mCCg7hWSEURn1MqaT%2BuONtMqGG%2Bv6v5z0h9LVLl6D35k%2FNdeEkCA2QRRuVUmzqj9UEtaA%2BYE2u5gyePhsNY1wgeZo1MLMbE9xgLCq840DuVUV6xqiSqbhmZdjbZDIsX8Jyp5ndYmznVBqBES6f7cwPuHm%2BdzOpn04cjCO765HEFqhTELfqcWo%2BIjCKbtNyaQ%2BX0aBl5vCbXVZj%2F6P1MVnJDOqMFA4N5H3jOH25EYGZV7ubxg5cGQH5w83LxSEdbZfRunF%2BEezpfhyFweFwdI%2BgpiEB9PVaKJCINOFSMDDfudLIBjqkAQ%2FGd8oJ%2FJ6hi3EYOTw7ULamRcgRxTT7jxz8hq8vaQRA%2B6Ov8UADVC9wteP06llxrLfuj44bwaLY8wSZVgKLiM7cjQSck82f5K2zoHHvym61tQVwru7ta3SOrADv3nE2%2FzxfNiUPPcss3Z%2FsGFNh4hFtLhhQA4Fg4mXL6AKsmwkUy4LkpPKhjIwcKWvKve6VlhPiXso9L2ojxh2liCn3nrHl82xm&X-Amz-Signature=6ca4b94c2bede23f6f2121dc7c59101c8d11f9331414a2d1ab6fee280bf286e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

