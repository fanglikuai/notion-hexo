---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVUUNRA3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDNAZxABxkW2snBI8y6IglOLNqHxq6jWweU7JWGsxCnCwIhAOpKwOivFEIfT48j7KlZtRZeYuVYn00oHeb4ECeetevnKogECOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0UvF3UdZVJ2nXq2Iq3AN8kOSWzCb0hc7wFonIQ6PTiecOcwjDs2kiRXJhi7dw8AREWAKspTTtkoiY0aVwzpC1cH3bPBh0GX98A49LAsBY%2BI0oHPthz5s2nJYZXtdy1yzZ8PVY4A0uTbMV87kXMNWo%2B%2F87hdaizAK9Ug9d5Q9XzA%2B4kAiBv5SXo7nL%2BU8FmKS9rqZCiKH6G1wRF8ukW8cfxiXO1Wo1CO%2FglZ2WJFQ8UKiLLxKgCZQbRE94D%2Bg6hc1MgLONybeZnouXSvwydjdVQTDsTLHbQnkmhUtvb%2FOs0RkYkpGK0XXlXwAzWfOKnlbIChUVI1FjkcpZR%2BVm2QUCHBSQkP9AvmDM20Ost7OhAfPQEqG1EfUbKBLcp0K8sIHkfwpnPoQlGN77XT%2FWYkQzNTgu%2BssP2ocaraeXFhzqyZMI0UFvFDyQBCK7gBEZ76Ps4GHKIMo7YcgFbOC2jwG9GDSecLjKy%2Fwyz23kP0BEKOSNkmzFZMQ%2BdQm4ArqdDC0YInWyde1ZGObshgWJhqq8dN2ddOqyu1dpWOZ%2B4ClPTYi5jFeIF30BxJxf0WFQSl3%2FPToO9sfQmsbvNZwmWq3DlXcF8YJWO7PssoBwv4TkFlmV5W8D3B%2BZCYeamVacwqW%2BP6CEbCjfvSVxSjDy1YzIBjqkAZ9DdZuP2dkDHssuKzIs7G8TIpQ4AQ2cXstDln18OhCNcI3EE6r1FcTjXi3nmfnIwFfLIQIvFm6V7bpRYy1BswrbUB23l2YIrMvp8w21w%2BZ4k%2BnrBwrxVxOXcmLS7lsueHxX4h9GIqQETncsipCovIEmbEUSws6qYSCYSToKy78wuNIQtJ8mGvvuYC41UWUnb0Q9Y7LcA%2Fdcs109UKLQYN6zW1On&X-Amz-Signature=491db86cab530af23223bd7ec2758d97a1a1b71b5e525d7151204d321342f237&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

