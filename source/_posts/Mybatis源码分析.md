---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AIK2T6N%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQCBpqazD5J5Hh9y2ftccOcjcegUHXxoLOOyQMCZz4Xw6gIgLJwHkFtxuntXkFbStrW6d%2FShESZZLtV9atINjAfjkmIq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDIPbMEb9E9UKG80Y0CrcA%2BqWAnZKCF5vCiC%2FisJnEueMN8Yxe6YvEiSj1Z6sDVmtbxArsPjwvvjT%2B7qJX2OOzb%2FklwozDtgn0qr9lf8ONlXONYjd%2FWt6iRn5GEv8VQS%2BGKfwCf4kkYnY3YtzuzzJjUSmboCyy1xFt%2FiZ88lc2WwY8Nfz1Ywaq4Y8oiNaY%2BZcKV5QXuB2fgidYVZs4dpQ1scjietk8PsVYLqCl99m4JLp7ncI629j1sO4DPDmpLLI7iyX2n854rBCcEzYzvbOfVOgTx7iy%2B0IBfSvqivYBmPHzWKlUl%2F%2F2BKq8Lrt0Pr%2BZHhUwgxv92lPmyIJuJIduU2TFMyvXPgj6XVf22ltVY5y%2F0c%2F6KBmogRAaXnerhtyF7Am3egA2fDAgY6zhpp8YiNZbmiVajtxc5s%2BDzsYJdjk6zlmCM9EA4r1S%2BZ%2BAuSBVnAZRMn9WJ1dNVr6HsLe0UwirrX%2FKEl43vsaJz5r0RAJjND0kBCGp8ZlHYi%2F2mZE5Bl8o5JnCgysZcMmjnLvD60IcLVxEhqAkIz5qhuSwkscggzpxJIq1zifWI5xtvEamcO8VYU1LSvBpvDM2%2F3ZNNAQnREY5boN6g%2B70MAaqGAWyQMvbuHoiRzZlASpzDKDy%2BjQaZBDFzBqvJ%2BGMOuUm8gGOqUB82JCyCRLYxyTA3D1Mfszl86qfwblr5itQDg4R3OuDjpiqh4g%2BFLSutAbjofkqdxYwBpCVBU2Oc0OxWFhQvun6aplCc63aDJfBy4tbyYnrO7wudEXFmoi2AmzYDjsQKbVBs%2Fs126kG45D%2FVphWU8PWTKyEvVvtZs1gE4bfJeP%2Bqh7fdaweuk8FQJmlBoVJnSTna0x%2Bvd5xTkBJxiAy5%2FQFt5%2FShMr&X-Amz-Signature=6ec7abc0a4ff083e038f99f5935da55bf9464e8b0e90023d9e3521eb9887352a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

