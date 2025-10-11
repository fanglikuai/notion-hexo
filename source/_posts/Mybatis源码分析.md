---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5VNICCG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDm5AoEbVmSY2p%2BLBCyuMRkagf1HjQn0A%2BRhiGnYvGyFwIhANVbtb2pOXmyjOePlPFU%2Fkp0o4urALOj3lAycaHH5riIKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwXR1yJXG8jf%2B3cjHwq3APmDdK06aYExddu1A05MGYFWWoCYsYGCXhE8UYrbkUsWmy8zYFLEOU%2FpFzrKTAB2vA%2FLlZFc4veUBSWNbro72vAmUGn%2FNQTCFBDfadyxUMGmPytcbrBXiqUshVYzAm%2FRHE%2F9b%2F4KcMq2fzhxa1U8huCNZ9mIHgrsDSuD3KteAzwu5yr%2FQJZt%2FJtqfKmjocbJG1mqwP9VXL6YY5NpDFb7cfx9xWEp9Ft2ytEX40F0f5EGKa%2BoohysYOkvdC2NBrqSFwlT8%2Bo6cXVtLyup700MYBH5JabOjr62%2Bn8R1TbbvujE1DJjIkA4GiOWhXkyk85dzP6EOE4ArOXy8k0TGvUTfmvaw1W4Jt3dAQMRkyMR27j9e5Pl6HjwPRPc69hhBmry4KouCheaU35Xm7xBXUpSoLLj%2F%2BOGVAqbQTGeEsjcjGFq50M7a6lZ5MshP%2FiErtoZgLzf4FUOmWTs36mob6gVoVoX1l05GM0%2F2FEe4HsUKeZliapFgq7Yv4iPV8rbz5LNHIdWh7Y9Ix7DLK7X6PQzWKki%2FOpb0ZoMyQ744rHFXitzF0n4wb%2FQf7J%2FG5lA996lZBHlfMI1r%2BQxXGm0hBwhK3tsIwY2RrXvXC97jj%2FXup3fD42OHyeGKnUqZOk8zD74qfHBjqkAfjTYRcl6T8eWlu9RdcqFjmoWa2yNrrrrTn1s9l3pafj0kZOYuCjaqJV9stwgx%2B%2FtFEUMO7KgkEvQuJN0RrA0aKOEoDi7i%2BjPbH8HHYt7mRND4q9BJ0TeLu0rRqCh38SgodaBp5%2BWp88SOWWFgB2%2BOpkCdCAiBH%2FI9mwaEYwVgEAFN46u%2BkCyHOiJAev6xHUuS2Ktcpn6nsLfT4eG89afQesjf%2BO&X-Amz-Signature=b977118e7dab40c3517d35a6f232c56ec3706da5cd7e9e6ee732405705b3a4f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

