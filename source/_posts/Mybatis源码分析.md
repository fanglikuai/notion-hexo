---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVUPOMMA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIEd97A%2BluO240HGX6lO6aBCSprSCWKE6iS%2BFT7S%2F60HEAiAI2SNTHts6kcx8b%2Fj5T24jVQ%2FgWHPUA8IKLHhdQIz%2FxCqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlFX%2BQOamDiUSQmGbKtwD82mF%2FJoPwEU%2Fgvo7IAl4fEyGrT3kn4mNWD1%2BMlgPe1gPht%2FrR8%2BkySap931dZGSu8IglM9zkx2dTsZDqRy2uA9%2FgiUX749Birmd5ZAz%2BM9XjfVAva8dp2Dez52MoUPhHSmIAA5JPv8puOCCFSba6DqjuW%2BCcnVRLayYEnxUf1JXqeUCxdf2r%2BaP%2BrG1me5MSPOxgf7GeJzKtS%2FUkObCgef8iO5Z6xpjL1DT%2BF3PdJk4XFXDLsD%2FqEScD7E9yd1QDfB5SEm%2BTVmQbQccUxwsZgI2tHdrQhzBb0zP8jVsb6Z6tfgeln6OjhJ1VNtCxbTGTsqisYR5doLzq9Lgx9yIa08v45Vd1gDnAqbSNnpTjkHRjEFUfbyV6h4YB7X5YfAsgeaW2%2FX5RKdD2Tg7InxfTOLAgP55rJVC4BvE6u%2BHwC%2BK0%2BlqAePHmrN0w8ZUcjgr7AOReWctvTl3d6R23sGfTpDCUMySFPKc6pommRz4ehW7hhtu3DYIQNDQj01aIwfIn8ko0P7KyfrFclurxjgTz9tM2JL%2F5QCqtNcrkXtH59fhTuIiS3GmqbcRkgVNIWdJ3sJ25FrcQNVV93FP4V0DYzn85x1x8k4Ss7dnmIcZvLK3H5Dlj3lm0wJsc%2FBEwmK7CyAY6pgHdseNyaQ4ghKgAI0CaYgvLxpzRwE9Qq%2FQxwhtBaZDF8PpsTZc42iThZ7GG%2FpNjE6%2BkvsHvXYSBaiWzi8DCPsvaFh5bfFZNFrgZbPcBZBoUBOBHNIYMwMKH9yHKPFH4I5TD3VucAQzGaSN2mWJi7TeaSsR%2Bu67FScuY9Vlt7fc20XELJ8zGV%2Bb4tphZDK17mpnNohDt5Nw2ds%2BitcdjeCk4jbGH7hwm&X-Amz-Signature=9f8e836d848e7efaf82ad4d2fbc435dc8d4bac3afcc30d4a92fc46fab914d02d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

