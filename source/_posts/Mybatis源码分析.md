---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665K7ZX33L%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIGA6XeLhuoMc3W0c4dXWUemqndsxlnvhZTb9XBYJTb1AAiABIvkZli4KPhOFYYmjkAEfeNR9VbxQLG8jAb%2FopuqbjiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6nUxSTL%2BDS3vPA6mKtwDK7pYToGfYyLLfqZSG8z9gt4p2LFp41HyH7yk2e8UCsOBawgOiUv45ECwQjgmY%2FeB7FoyPmcS4nKwWC%2Fo95nYBQ81topsZFjDTrMmVvXHAjGgEnDooVGL%2FxJDtXaAwsyTGGW68JnNVbLb%2BMiIxb90fkuPMhgdImk6Edou%2BU58Wo7IeKxv7mPXiZ3AxLagJlPiUEQwq49c3fEVTsFH3XM%2B5ZOZt%2B7cOClwr8JxZwEgkiwM94IgJbNFQ0SrMtI0ASFRU1hbqtcO9TtXA77M9gj2o%2FumKSVXorLfcueqd9cDVeRSE1qOSz4LvKP1Z5Nto4N8Zmna3IcQD6ZlweRRxgGeEo5PhEM0h5T0SK7o4ewxNTlqVPJp24wYnHaKkFFdgiUA4dOUsKob%2B1WI54pdjIHVwMY3JWHuChX2XxfC2iDPmcU0cvxDc6vJulGtE6LM8uRvXtahxjsZFd8SCvO5xqwiutZs79TReEp3GuDCAmr%2B5OwWnnWWPWXJMW5ubzcVC3%2BGjgSHOAlpeG6C55z8Cyzv1B4xxLxJnmmVGy5oDZGAlMoEQaGNrUqQ9NcZSSZV9klYJoHN6vaRu5jSJmmLaf5iRKbjyiMCH2ECsibTwNL%2FE5HB4Zj3p3jbuEnjt8kwkJn5yAY6pgEHm0O7rcbwAmWbPDDSINojgixDyr%2FWVxMsrbZOCNoSN6y9ONEYmewRPB1jUfBiPGpMCVf2uF1UdXsd2A45Xw8sA1Z6CsRNznEQ6S3oLXr9%2FUhT0xwb%2BM2YJL4LX465KAN5QPp%2BUi6%2B76l%2FaLqJo9T9PpCBXof7iaYLZ2iQknQG7OuHZplhQ6n0UFvbB%2BaHpYGn630lz%2Bhvlc8s4VhEBNDbOMgDIghn&X-Amz-Signature=10a37cc6711bc407ca64157cb38a813937993c3417113c7889aa9cf2f8fac072&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

