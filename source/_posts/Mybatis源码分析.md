---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZLBT3H7%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJGMEQCIBue0SdHbQEMhyrwxp6jbma8TrRZWMIIIhuVCJdRxscBAiBhUXqM%2F1yuDL%2B0K7RdPyrTzWkL0NdOQZDwJAWSWAIIuCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMV1BiyZVS85ts7aw6KtwDS39wm5jxVzH5F7DgTnSeOrLH4ITJ0uPjiFyZkW3aXTqY0PaV72XNNF9M2FEnQ6aJpZsimg44hJG0dPODVixNSW0TOHNGIoTA7ZmaQVbCLd6GwBjYfZpJr1HxfUxoNOjpS9LPL0sP8fAFGkSI8vm6vN9Am725LIke0cjNfXkgZPjRP8omcqvpMwW4dfCFCn704pbE%2BIwbjjYinNlnbPmTD2F%2BpZpHh3DIdWcBrDp4JeBKOmZ%2FEKrWBrbJSMNCPK4RxAiml5QheEJ8ny0EmsIeLt8OSca%2FMBRYz0KjpGP3PkjeqzRnL3V9EDiA22xj5VXQNCkimq4%2BVhmj5WqJcL8HStn1I9Lz37MkVAMYVSlIdHb5Db1vczLW6t3NPqnrdtj3md70%2Bj3au5bSl9z3ch4vqV%2BGvi5Gyj3J5lSmliBeoey9MtxPVBOpSnsWxlUvLAQFKDB7%2BLVSqv%2B8E88%2BpRCAigasEdEF6y%2Fs2GDbuxIP3kHxYrmMxMjOUmL76np7hqDahAw%2F9zGd9VoyDsODH4F8UMEI5OqFa0%2BRZzDQMG17gi0iLnfUhlQe%2B8P5%2BqaH7H49144r%2Fg5zkrOHWuTc2QEfTsoNGaWzl7nquR0zNPBW2kJxS27fNCDwjJk3JNcwvbTTxwY6pgG78vcEigbxd1cNDhgafh3%2BOXK4QmvQGLFNaimqrS9wovWmzJjBk7a%2F2WPyAevuqCaTGS442lA9edfa6AMerbvlHQ8u47kkFoELw0s7hCIPVq%2B45sEKl5E8u2tZJPut2IeRWFo3MKUHhiXf8EO7%2FtLUFEkN14UuAf0x4cI9f379d1PWApJnIA7Uzp5EVxGt0v4I%2BjYxbRK8ig8xPkiAyUYHOciKGEzz&X-Amz-Signature=9961df7d874223c69c079d10ab771d496f231f580808011d2cac12b5db9cbb61&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

