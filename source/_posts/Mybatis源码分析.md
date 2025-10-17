---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCQOOTKV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BQL3vvAtjlAQDdWEVTxfFKJ%2BeZ3%2FD2s1s4vA7I9CKKAiBttWJ2PPNd%2B3hMYMWKeH5NCnV88FdDhyDOcbDCPXbkPyqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjnATRKk94dUgn%2FRyKtwD%2F91HQK9JDIPOnTDAyuVRfl%2B%2BRhG0TFz3L%2FlaJfLEfSIuuz4ny6VFQnujCmehQkn2Evej%2FU7SxVf4yK1J4HPE7ZQ9e9%2B21zqS6ULIeW6hEA4CBpMUtxLZvG%2BrH6Zb7N5FzPqW8kwtwQsfGIfohi8YnRWddMiICZleTNl6V9eUt50AW7cC8MhxhajlwRLk%2B%2BgeX17FtbX1CVk3zvA%2BHwVMjNLFuJe%2FsopS03LEJt7t5SBUorbNI3SVeV7D99Zg9hagruZpbeu4cM7Psk8tm8I676L4ahli4o2kojJwQSShp%2Fv3o2%2FHrsA9JMKifB2eGg5EitEnHpgXFrZa1JEiC7Fevpv16raQS2%2FGcVwYX2pyxeZRlFuDBzixHgG1L166uO8K1Z3YINvSAWCXREOBOkekVtbBAE16he1lDvz0kZ%2BE47gt5OVy0QLHI58nQQy5dc9npbZCGM%2FPasLkmuMz8N46nChPnZKViNS6qBVMNwaufwv7n5CT%2B1bYX9lRN4uDbhwjayn4ytLzKtY5sW9hLBvAMe%2Blo5dO0RvGdyS3P60mzWPSXh4ZLp4rhRY7iLCOHmx%2FAGmkM3qTTg165lTIG1fM8uZKqpg%2BsNDscxIuQtHrf1aF8EHVoNiCXaO%2Bm%2FQwgv3IxwY6pgHXas08hAUq2zorG9AcXiPoSeYc3L6Ys1uPOQroFDxLi%2F0eSkhDKKCSTCAKuh3pnzVanlyH%2FRKfu6GvwgghOS8iCEEU%2BJHUi9b%2BfTNiBectboM5PBPc4HLCysot7rLr93i36Jol4gwe%2Bd%2Bd9WYfz14Flrd%2FpMJZbubLb4%2BFvjmAKLrDIFm9J%2B0%2F6TiQ6hnEioy8TDUun3g42XSk6CE8NlUVm7Tu1YWZ&X-Amz-Signature=a3b16cc0178c6f0a0ac5f7f89def691afb3189f63d91f0cac294e420d7db7d33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

