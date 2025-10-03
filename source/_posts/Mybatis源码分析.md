---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RDC3MFB%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGUZyMGcDQ6D%2B1XyK%2FbIrbq6FYdtczM1%2BnP7TcF8B07gIgGKvPvcPV%2FUZYWxFlH%2FmNsbOddkJSUWDlfyNbA3Nbs90q%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDKn2pPwwCbhiCL9N4yrcA3v5liUcLNj%2FS2KZyd7Jy6ApCVXRYq5CZLMdsaZXBsivetp3Gm%2B5kfWj2fT57e80ewyuqfBFfCXr8jhE3dnQ4YE6PRU9ebKsPZQF0EzbINIy7q1PjsZvSKag%2FKp1GnDbbCMuG1cO8e9io8Lnf%2Fuf2cwF0oRFx82omo4COTUc2zMtJqsCGW7TkgupcO4MpkAe4jAeIXV91Qlcp5ZUKQGqDP%2FjrZpmUI1EozWi04G6S%2B5egCeD2r4GAwTNKn1RV1APAjLaewrc1unnUXcEK%2BGyMxptipJ4ORB2%2B8gQal1aLfs3n2Dk0nlWSJnKz%2B02b5gtV76vg3YVYmV3BepvxoiurGUR4ACd7r4MkI2joWY2%2FN5ez9jfSBVHTe7cAOvo6mjQ6i5YmY0FH32fQJt%2FRdGxfNtrIMCdwQm1mwaIprUaW02V8IviY0yKtNZTyhm20OciR011IoXLJkvuuqKSKSZloMeVqah9SMJYlNzzOIB6djq9T8zkSDxi2jTWFbt3qDdv5FhtwglaUBE2izn4oohz9L%2FmJFw1uUhys%2B7TNlUY64H4c%2FwdshaBp0b70bXfpDZPllF6uHWlW08KT4lFbMzjL2QUF8BbrLB2RO1X2X79QY2r0yVZcZn7xxCWhs0yMKbx%2FsYGOqUBUb6d3s3YQy9zcxdUFMeRGLmJtScUoqbXkpEInJYz9T6aMToEUDnU1iecPFgPvoTzCCULT87hRMDAOvCNE5xIWMAqayfRj4EjEjJoLl6FAxKSKdjhtPKwOYvoI0478zgGHKVnSaKP3AjByTS9BdyNPWKJXCRcY2E7OgrsumOvhd9WKNVa6YOaCO7xIyTEgaA7wqpx1OhPYed3T%2FHx4YCPGvyd%2FMC%2B&X-Amz-Signature=7569cc6018c16a3341a204962fd4c126ef2a258a71709f33827027618a894316&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

