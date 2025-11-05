---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQVM57FM%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbgk%2FIhtlkA47dRDkEkp18EUw9ogsvao4pdjcljMH9DAiEApBRG%2B%2FULP0KU1Ja%2B%2BTO92NYQF0uxVso6t4%2FPl1xb5MYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF45NhqdlUwUA3W04yrcA%2FEPovxBCn4y07Z0UIz6ivCOsbDuOuFjexiCU5pKywi7uKStfOa2MyUbQlfs0UErMweu0YCdZNYpUODk1XAffclPmd9iZ3Z%2FPQQZ0IOPgE11dW9PTGoxLyHZWvk48c1lwdj1e7DwhNGdI0%2B64L4ZU%2BguCTgKFfQNQWcEZBh8mDAoMmf66hfgV8Nxjis5ry8Lxzcv8Fcb%2Bg7S7ndcDwDeJ2qS0W8k6XkkNF2fWUcWJYsbGrJaEj9tK7w8Ci2e7yamKzVZDNHy06cfc3CWHD0Bqhx8NYtVhp48zg1zvOjPqNQA%2Bk9fpQwzdKKDNohNZWdemEFq8czVu8y0cqjYS%2BvJHoPLlT91S0%2BJu5GvdaaowHSaukfEXhMEqya4RrmUf%2FakCs%2BPM34hlXABhy5Ab9VCNisp1S7kb4HpwzzXC%2BNO%2B930ESyvKh7UVFyPTOYB5L7RrGExWtticeR0fhMiL1a4bO3pnvn4m3X%2FdYOZYwXnoM7iZoaX8eP4KFZIyzE63d4kG0vQXcG5KFnK3edrC6bHGm7DP35kZtB2pmATZcYSfkoa7pEfsWiTjo1UJZUcRp2F5r%2BTJITvKSjUnkg5h8QQoxhJmzUU3J97hDQWOou6%2FSwQd%2Ff9ec7w%2FWJn%2BawjMIeGrMgGOqUBCADOKsCn2OUvspJnP48%2BV%2BnCnHEBrSzymQ%2BCy4ufrCkJoHoJ4T2T5d%2BjG%2FUGzIpMGcmNnSd1u5Sotp0y5EwTEYV0a8h4Hx7iDRvBy1r2PLxYpNn9qxrQa%2BKcGpLJMefwjD72SQwsuwwL0SV4hSlT8VLsDFoXaYqLVfuAUVWKlZ%2FSu5lkBx0CYeNaOc%2FBFj2BPb7ynIdHvV4paElNYiVPyWa4MNFy&X-Amz-Signature=b0e54dc108c90e0f6ff706f14b64633e6e214e4d3fd325f7bac5870f0134661f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

