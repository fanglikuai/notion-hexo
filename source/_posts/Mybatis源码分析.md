---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQBYYAUJ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDz05JrteQzegC%2BUErK4rMSRdDg4ZAmBnpYiH3xMIA83AiAbgwjhhcnnCk%2Bh1%2FOrC06LKYeBHsNpIOGmU8F4QwjsrSr%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMqn1cPql3IDM%2FjiAfKtwDFNhv5R8y54JqkoyjCd8woluhauSafSVrtzFVc5iGV9hP7bQ2Dlz7huPSa0aSaZ9wsvHM3ybGqzDhKrgNmOmvQHzm68LYaAVRq4B%2BqQS8jjNwwhQPudH1WNOya0rrhQMfW%2FLYvhW14dJUzSC09GCFdFJ4RpGO3q43NjLsFB01b02i%2FIXG3dYj1NM6%2BgPKp%2Fe0Dm%2Bmi0kUiNRdopDFmKysLjrUn1U2DDWxmf88XVE%2Ft6%2F8BGgaJdSo4lFOrWWbjfl5MvCQyxj9wqAb6g8v1k1sHgagldSE2YlnkQEpGUrSLfutwgub7NSa2l3Rzf04R1pfjEHHGA0TeW7%2B53irkoscbN4R8ryxgX3%2FpuRsEThl0uxP02kmxGTfUnjNludMbY%2B84Uqe6UZ64nisWxwkoGBWXdiy8kecfKLzym5KI%2Bf9NR6P6WZZJCPwfXqdYG3vCSzAgpyj7d9CQa0j5ZfzrRRbQV%2BEubVgO77THg3vLQcodrp86%2Fdh5n0cBa59lmEUQOhHzhSdEqwB6y70UnsiQ1Dljfwj6W%2F%2FKVKivOyt0BgAJDMBHBa9AnZmfmqhp3YWd%2BGGRtw6q5gA2wXtCQgOMBaLPPemcfKRqqCrZO5MWqpzot6s2hdTU5yFRuO2Ixgw7pOfxgY6pgGPn9p7MEkNZ6C8WKs%2BeU%2FOBRhuU16MSgIDWfAE9er%2B3iH9KQSIyIAsnpfsdi5%2BDcYA8XitTgV%2BfV%2B5DEykffk5DR%2BHqiED9LSpmJeg90mnfaYQNow%2BPCYL5plu1c745eQ3gAyN%2F0aE6mYxe5jikoS62lHoQ2Og9sbXg%2F98e9E0DXlm%2BFrxQOSesw05nq2Ns2SPy7ycmq7A7SfrwRjsuaz%2B5IDd%2Ft2e&X-Amz-Signature=bb6135782b7117a9057b512264753f2ad34bf2c864a2bdb2740e28a313b685b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
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

