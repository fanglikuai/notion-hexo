---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBY675YZ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmbSX34bG%2FY0KxIUaWpi38bbRyPGMN9lEqGD2VzrXxDQIhAI8NTBPDo%2F2qYR3lhCjY2uh4QV%2BIbp75ZWkIuiI40S2bKv8DCEoQABoMNjM3NDIzMTgzODA1IgxJJEy2STtv8NRAz2cq3ANuI91DMgn11lRV%2FOWJaUzs0Rr8N%2BStxqdPyLXUWqsDotEDdUNFjvyYoSAZT4%2BhMUAXnW9Nd52H4SkaXowJReZf3MKODuTAE4SqJSW2GFPRl62EYNldmt9TBHdr7d0rm2j3XAqbAY85jZxNrG07bXmKYLunIu2seIfD9CjKAqaJPwKBwGNg24U5zxkG4Rf5OF%2B7QCC995aTaf0kwhATxYMAkJ4VuyqDbv9s1yu4JmdOw152%2BVcYlIREMLjc%2FloSp3TgyEkGh0zEnThjp6XxEQj87GEvaNx12ETPiMAl%2F%2FZnbq0B6Iqo%2BSAkaWTvk1DD7Ybzah76AD01AORXt0jQAHLJGO4Pd%2B8h4JWvWoesJjgVJuwQF2%2FovcoxA%2B7fDDwVwitR%2BVs6wexb7FoYvyFL6EmhUBHkLDKA2i%2BETeJMvCyUKZGABaIjGfnsgOhg0NmX0cfbrnetGtTFaX3sSVBwEv%2BSziSbHaYsGAbO9hIVgDuuA6RUCNjtr7Wta9qd1LjwPjFGzK%2F9JQ4EoZVAqOJwTEXRHXBzo1Bwf2Xacq%2F6dR5QjvlTcmyugNZ6mZULC5M7ZQswTTNUG0dStkDSMj9CDJUUXAnevSZdHpN4579VoRmnKM7o%2Frlqyn2Buo%2Fr0TDSmsvGBjqkASNUovOtCu9K4dkFM2t06bL552s8RRwSGnqupOoNuxZLhFZ%2FPrxt%2FfAu79%2BlMiLRj%2FVnuOuTiK1FX5%2F6dzjQMw3ngCDLRT8nHhu%2FOrW74H7C3wqykIKsyC00bTM1FXn2DvGg%2BaKgOUfGOSq%2BKc%2F%2BTCTPsim5WYfQTNbOdgIEOeA%2BJYJ5tnEzx7gDeMynNvnmKw0iKvPdrLT6d8Q4aWXGdvehv3K%2F&X-Amz-Signature=58ecaaffc13857ed839c66eb891bd697fc57c4f4a6853538cbaf380f45c0a27f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

