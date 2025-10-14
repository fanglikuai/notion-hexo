---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KBIJHCN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T130038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsN2%2BBbxvsNyw1WJ7fec2E%2FYBpAr06Ozi0mroUz0DOjAIhAP7Cj%2B0WsK0glttbyoyffetJKbEqfdIZAerBYUNIHXzYKv8DCF0QABoMNjM3NDIzMTgzODA1IgwyxCuEUjEtt4cTplgq3APm9tr4sGSLz2Nand9PSg%2FxUCr9okr8T1mHXgMd2Fc63xDtpKaXpyaIZoitugwHyGOvdWiApduYwfVpYU9ZEP2TnWzsR90WFPaZRxVGNngmrntRni5rhWEKP0%2Bh%2FI5vlcn88fTnanrEuCK44EQLMupdOPRJVPlgiGQ%2Bv60GZSkv%2FfbI3CLWrTmZNS%2BsilOXZ2ZPP7qnZ3fHNo1Ohpl4g%2F%2FkxpmlW%2BPgDwggrclOibuQfnwvGx5xmiay9lm1e3GaYDTW%2BgiuvIDTisAvcjEBv0bOM3bneAwq8bbqPit1nB%2F5%2BAAEiCXt%2F%2F6aLNaR7ys2yd2lGVWQ0fLWgtKKI1SV7YlnDsAi1RCN%2BiCy7QU43kAuBDhZ0yMTUmyDaU8cJqUJLeILFkxLwY9XASwgQk42SnYVc31CmegBMyeM%2BbCztyzJtjbeAmbyKkZoHC0sVQitUodoLOEUqWcLn%2FPb2n2sjjbBBQ8etvTfoQ8H3oLZCDH9VRxlJ9cc53j6yE4YLrETaBdQV78x4EzD8Qa115f3z%2BpP9FaOeZMqzw4tZ2EfuqfcchRhRIaGfqBgHQkG8wMvvwIs76ajXxqZC6Jmlmwot%2BryojkvX%2FzTfPcCsF7dxrUQm97lh0yopMxOGqpIgzDm87jHBjqkAQVqFFkut60GBlFCnE5ORGe%2FT8a12mc8vpcg%2Bqi6qiuT66IMu8xeUE6qvnb%2Bbz6AcS1VNHEAFbkYpmgSZDUHLAwBEY1%2BV6A9bombFpmiN3ZVLEAGXM176l98VFEgWBpUg1APH04qoSayDI1mbhW6jNZy%2Fk8UgVTVfz0UuEhmWQwYPUSuptKBN6r6xOBRisWkgSD7aVtPDBBYpKSmHYkP7BXnvryH&X-Amz-Signature=2f05836d56184dc9a7e013f67c84c8ab677e6464fee36d38effedd67091fa81a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

