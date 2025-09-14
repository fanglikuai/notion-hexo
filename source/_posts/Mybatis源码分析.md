---
categories: 源码阅读
tags: []
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIOXRCQZ%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132611Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHi3lkeA1OZq5h4c7FJF6FEMy7yb878OuN%2BlvUOnnXQ2AiEA0TvrnB8pKNUptx%2Fl3mfkj1HaWqTLj8fzJQ15ybZ4Iosq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDOuxY8Y2MxhTm0%2BEYyrcA%2BPhqJ8KrEpYxi%2BWUoUBSj8hx%2FzmaqGWxyNDb4iIyf1rkNdS%2FjrXkJvJQIjxQdNwG5z5BoVQMZHH3xVcKCG9Z%2BDUOTjSv5ONmHAoLZfc2m%2FG1vWUFHAZRFSZtzzDX3dRSBcWPuKgHafapHeACr1%2BTnnKIxpiJ2rRdP8ugEagNfJ4oRps3p6X2RUXXtFfEf%2Fq93KsA4KtqIRju%2Bap1CRIrFb9WCoMUjdqChNI5u4UXVPLSz9mpUOp5GFN%2BrI4lkcg18G%2FEhNody2zagjm4j1z8VamYSmlPiUq8VTdAzxngdH8Aoxt6OdgQWlONcFoFDSZ2BTcX%2BGm4kx1WIsnonGwSgfDBgLxqryqrcq8cGFE4CLMX7HZzbL%2BnpCgAILv588i8WK5AuWwqAVfY5XxocRUqoy3NY3PRMEwxgDlo%2B6Rx0QMe5HpOREpTbBTfdNmSX10wuNJBDPGEdbkl%2ByDXnYhatrZxwuwsLS7VP2LtPFm8sS3zP11Ke8A4AjmqYWHXJMxWUZvJSX2sJo6a%2FcZw9bhIfN%2BE0epL76BUdTvATaaSY%2Fw71Hpxn9rN%2FjapRKX0nW3qZwdwM8XNYjzkZ%2BW37q2C2D75FAQPWe3lwLgZdESGmEagCDKjV6qr%2BsFNH1YMJTsmsYGOqUB570zK%2FXIA1Evma3kmBYSzBAiR1hRX6jtbk8szCxI0WE80fgd1DhJE3yCDvmuzslkg3jCR2o2%2FEJ9nuFd%2BGqCBmBFKN2eHRDJpwpso2DE9Kb92FDRVmUUjCkh5fvNpAaPKj%2BufgN1jfXxNzN2bBWD3sMg7wMe5l9655uczzjN0Eva%2BEV1%2Bq41ZmpDpgA%2Bqg3nSq37mpQX0X5qo0Nct4FWYQgp0V49&X-Amz-Signature=ae2f7d3495e7d2ba0e3aff87e1e1f01a5e2cf7346b65d40a339216b827fe6d60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {    public static void main(String[] args) throws IOException {        //1.读取配置文件        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");        //2.创建SqlSessionFactory工厂        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();        SqlSessionFactory factory = builder.build(in);        //3.使用工厂生产SqlSession对象        SqlSession session = factory.openSession();        //4.使用SqlSession创建Dao接口的代理对象        IUserDao userDao = session.getMapper(IUserDao.class);        //5.使用代理对象执行方法        List<User> users = userDao.findAll();        for (User user : users) {            System.out.println(user);        }        //6.释放资源        session.close();        in.close();    }}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

