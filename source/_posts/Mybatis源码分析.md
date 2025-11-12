---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOHQYGWU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQCZN%2Btku3QtqSUJVty9PsgYj%2B5zbIQbBySX005LmIBkQgIhAOm78K7HPxI2OQXjqpEfH0tFjzDIHUgM3Ts43kWSPlchKv8DCDIQABoMNjM3NDIzMTgzODA1Igwn1CxNRbg33YKvRpYq3AMM%2FWCrbVlL53uGpQ3xG%2BdpNIBJDJ745vIHfiaxyxuCc3khg7EgGbgoYgVcaI%2FzR%2B2wuPUQrknzHSBQCdh8cyFW4oeLXxtFquAoFdZz9NLOjdahHyZ4JNIfqDtpmlX3e7CgtZCEh1Rh0jOK2B5kKOGtl3KxWjQJWnni4dLhsdr8T9Cjy5B6NEWj3rO3fCquNap4%2B%2BKV1AKL50uIX2IN2Lx9MXUNVz9VQ4S5s2GxyUiQ1XF4LCGerqvKVXHfgnztXDjoQ3%2Fpw2LLX9BjXjzCtizRjG3gqdLFfZOc19%2F8HaWTtkg3DmZP5%2BCnMaS9D9kRl9kpjH6OuM5Z335G%2Bt5bGS5XjZe8nD3rxrENr1sMMepa0DZE9ZmloRURG%2BHf%2BfnNRwLrZZxIsUa1%2FoxcVDtFu6rRRUTWTsaZwT4tIHED4sWabnU9aCs1J72HTgcF%2FME81UBy00fii0xUxc%2FElXu1VbeXV2KdstN4aqCrLfs5kachxwYKodBgZGXK%2F6qrMPxw3MGDKbUwDL2jEZj4CDsaS5CrvJnCxUsKgOzZb%2BkPwbUKfeLzk0cs9wXzsK0B3dNxQy7EaY8Nt%2FWg0kpy61LJafnRzms1dPTN4nVkJNcoJt1MdkU3IXF1t%2BgvKycKpDCrkNHIBjqkAZTeK38YvbwWIPKWWjc9VPMVNTWP38j6PAc9Uo23ETVWwPElA9zPXcktjvROmA1zd7VG%2FVKBGxfqcV4kRngI3s55KTk9wnxwnUjqOW1NxlI%2F6Zad6FACj3HPQnF%2B7Sesj8eoqIryMiTLZWt%2Ftess2zozPZ9mTnyop21jZkuS3Z%2Bb0EsU1n8H4UwESPSf%2B6Q4IWtjbRRdJ%2F2XePkg37JbQjp%2FSWMd&X-Amz-Signature=7cf799895222979d301a95d0c05d9e6d10d6c442bae730c933350def421b7954&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

