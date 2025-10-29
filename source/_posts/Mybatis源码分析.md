---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQBENZJ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIB0Gzh%2BGTibuCtoMdoitemnITY3CDIM7bVPufStkHaSNAiB4dLD4iAbkY4EjHmxR46r%2FPijl8uLey2ctni2Xziaf2SqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMz%2FrmW2VQFDYPau8FKtwDscgLVM1OVM%2BDsb2WvVMugqZekvaQcpr0Hu14IkTxt2LJZxq3P3cMZUQuTx3x50ZFQY2zKTcGY4ChfBhDPsKvmAO5oYQ1n17h2pb1uST9qMq97%2BrMu3dHztrV4BD3qQADP8oy4idNazeiB4uNROVXxpgnpYeaOzHML11kwbqdyv7JmzZbOErwpBAumeFC8INZ5Lybs6X%2ByqP%2F9MfqQFPCj9E8ecA0gbwpLgDIXKnz7IZNYFghjSzzSfnZt%2BZ0rDtcEUVTQG5gTJsMGVMKTIdOQ9HF%2BIbYhGiAntmcqXwuG4XgJGNJ7dbQ%2FcjjgQA5Cq2NYmOPHtMPYmK0ZMb0WTaJMifih39B8EfeukQYBrorJrDboPjlbF85C9NCOyppOwfEpmxTrYjUGOdzKg%2B4GQBVpdyOG6rvlZp8c3qkL6gIxNdHFiXl0jcGeZC1Kk9A8G8DsuTICsPGbSYMgiulSfn6SJyDm9%2BXo7xWZRWP98mh0EcWEoya%2FYClVVa4hqsoAgmhHuHU4WA0%2FnOcsBgSrm1YIfmAUpTbOsEADarwxKDy64GHoQ9sQXds43plEd2bFOz%2BNG7c%2FjDojM3plJMgFJhjgdbGkBTPwQoBAK%2FGBhFfyhxm0fwONto1XtgnC%2FQwqpOFyAY6pgF62102hdVde9vlo%2BFsCbp8i95xukRQjbB%2FHQuOToJOFz3omyiAtJ45dMwlWeYuzIOKvJMs2PI8q8U4ooTPLUz%2F49%2FmYnWBnwYNxgPPn91hQ3FK1%2Frc0ON4hvQd%2Fw4DPJV2CJu3AerDl6k9LOUNoaFHHVzbg0vYWggTpEJeHvtRHnvuOdd13OFWwSXrNZ0E4y3LYOuu5kyU%2FP%2FDDEOnjLTIEAmmpYCC&X-Amz-Signature=ca915104923987d1378e89ece633d00a691d0749a32ef92668564c78d2dcc143&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

