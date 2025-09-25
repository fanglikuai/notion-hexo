---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VE3IT2N%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClLMdATMIKvdmiFtdVOWgSn1UiZVy4pPhopKLMd4KiawIhAJ6m%2F833Fc5EoY6UuCITpgLfeOmjLDTGp8rCQrWnAOc8Kv8DCG0QABoMNjM3NDIzMTgzODA1IgxsufzjMvsByut505Qq3AOT9qTKS%2Fidhe7uT5AAdFfEzRuml7v3ej9%2F6QRigkjIXrP9lFEENSGgNjC3SWYEX4%2F4wkE60c%2FEy551qOw%2BkVKljUiIL3%2BV0GkBWuPR3HicjcmupAJ0Yk3jzwfyEZxcR0L46osQywcie%2BbPlTty5ETieqCGYkeP%2BFoB0Q5bABzb%2BVaGXfTHypSRlCA25npgR4BswyC7tkQZ4Xnf64%2FG7a7SQy153zviJ09TjMpl6a6aFj5AG0zzSFlFEHFNXSN%2BrKY1gBOED2rQhrHpRzqheIYkhTx%2F%2BtNR7o4P29ecVvFtB4lICvQgL0wif2PjEFExKEF80%2F6QiKCWQxBZ5xTKkEYQMCTD9IBoLQ1GQvYz%2Fwe34WSrAtgJJLtfpxOcdFzXMcJfKiBEMmXXbqIoqU%2BirTwU4gbyfoujCnAvcZ%2F6Xix2tygSmXyYKJPfcEhTsAkNNU6zw7kuCN31%2B7Mxd5FNx5kI58kFqwxtCLc09iIy8hQSNp8fkw1x3fyf9Pa6IAJacUVgWGirOfzK6tsH6Lv8bSD%2BaU8HO%2FHbSHTlKTYbAHCNVSt2M%2BujZ7UQcf8XGYy7W4NBQvcHydTrppYLBq0wPbzDZFlJXDSjpda8WNCPnTeeKEE4qKdGLs36GgNC2DDahdPGBjqkARSuRhjajxnVSUDzBdmqLXKSOpdXQo9X6gBkgxSxywEuULnizLS8Wy5%2FsX4FmwyBKuLQP3Sw9v%2FKtLzyhB%2BWsAfoRNbfzlahgtaCWzfwEJ4eg9GOtkUmu5MJntrVSRaVsbKLPByIkRgkXk2tVg2AvrHSpwUrR1QvArwWflxOOWfspOLTRuRej0BAVWsBnXS%2FWTMedunwpnRa9EJj76M47DWSNrmz&X-Amz-Signature=a5722b8f31c4079d27444be7bd99a7dff77e3602f0b6b9cbf9508daa1824222c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

