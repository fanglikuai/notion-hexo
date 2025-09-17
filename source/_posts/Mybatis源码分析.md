---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666U3GXYHX%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCICHOh%2FymvsD9FVx1wL8wsEbJ%2FHXH%2BTGvuD8qn6I3G6vUAiBu%2BmtIFRDGggiSvC8nHRWDaMRtZs6mFfUA5R8gC2BFViqIBAic%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGJYXiHjuEmB3Mb04KtwDzQSANnLiPo%2BCNJcaQfkJma0Db73EWFsiO3fnPYnu%2B5bf7mW7SJxVqjPAXOsITyIGGyCL86p6BF385mGLVxUySgQYVzLyIaGLZRHUs3N%2B7lFKVF7OePUbcw1l7Vqq1Xssg9c2hLVQI4QwrdBBqpDIHkwGMiIbOjgV8WZn3g2cjL6KQvGa%2FYO0g7CHzHDO1S6q8aoaqKjgOs2MIaUpU0o%2FtgAYjgnPPQRhGG2h1LrlgfMAptQef3XkNGOpVpL1qaLrW7CoomGWRvTHQgXOgeDw0LxWXNUP81clJpsQFZZhCnDpwl8PG2loz82HrXl3j92w77KCTCdUmro%2FRv9%2Fcjofb7ww2flYLB4pKu9GMoUhedWyYemh3d3PwjqijNL36cIbIuBwUeiN8rTOtMgi7cXJ44MiIy1GbpBPBVz1qY5U8E43%2FUPIwC%2FrzCFQnlPRPSwLA2wsZXmiCfM37DNwCP1V%2BlAZE9Rw%2Fbvectt57g6aOuuUzlBxsPLdRpCQ6ePJVub%2BsiH12Aqrp%2FCTruriMedUTRuPtUGQiL%2BPE9c6VeKkCIBo2FCtCaPJkNTh7J9Qxbl4v7R%2FELAFspensxZCDX2lLwwvIVx%2BMZxRXdEZ4rq7Qe5FUWgATpIlt11rrb0wgdKoxgY6pgHFRw3SPoZ5W0bidnYYu8aEbZ2g3y3Vbcl5%2BuUxgPZxJbJTgVX7gm1g2FSnPVzwk3wPSG8yrFyRjM%2B39djr2jjchfiBbAOOwe1GJS5iNd5nuOl8TyxjVKGUX9HXZ0K2k0HOLLRYtWRyJ1LgCtDbJPWF6ZhS7wRdW0miieBpNYut8Kbf2e%2BHT9H9W2Ilpjg0AwEetGPTe68jY4F9xfj%2F47wM2Q2ChU0n&X-Amz-Signature=bc5380daff870b0e39f49dd61109c73e1c0f44d73ef1b8182926cf8350635f8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

