---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662TWRCK2G%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIEIXXNu0KACcBFq72nIHzNAKCsihD6PBTzEmZZzaLFhxAiAw5ACcs6qfdzTEyyj2Owtq6akxktwUTqqGTopm81w0bCqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZJuoUs0gDNdtVwooKtwDwiVYxKoMaJqZS%2BZohTHgP2IGXV7Kt3jdNoXYk7YESJdZpIpmZ3hqwQU4AGQLDLx5%2B5qAJlOC2mdO5aNsqX00uBDj23YbJbo63%2BtzW3kEbFI8a9LOJQ7llfBYX4TTabuH4rH%2BXfJYsTFqWIinaCspJ%2FBCaDn86rLj%2FloYZzem06Foy36un7O37P%2BsAIyH8Fm1646bs7tHs7zPgnBSvrz9%2BApi37eHAuYprOUpotygB78%2Bp7MJ%2FwLz%2FCU9wY%2FJgyKxkf5TC6ciRDwnMSFBFJ6pd%2Fp30gAtMLAc0KWqgyChewTXMXvz8%2Bxd5m4%2FK%2BFOYvwWckYNgiEDCQyZOhjm6%2FczJtbwE1M4UluT8T5uGlj4v4umuudOyzYTUIiWrp8SNSEtrv3bKfctgkhIAaicT0kbe6jo3z202%2B0%2F2UhlwAigGY7TyeMawN9lGlXx4LpGh7PN%2F%2F3ExsPN43F6lafSiv%2B2JTMXsAsOh81dLf0yoO%2BBRrhCVmjM5bV1lO44X1sPzQDHJt7T%2B9UyIsPqjm5X56QI5qJacAHs1pLd53vIuihmPYv1zOgkeUVCVYUJ4A5Fkx3BtN%2FVwbSj7n%2Bs3o3xqSK9Gh23ENSmZGrfo4wKBCfyfv3etST9qkiUwHzF1Bgw95mtxgY6pgEQg2zcaNFGvHw8RVKBOqQGIEW2n0SDr2XH0aXQ2DwmyZMfnhSvXMxuyUTXi%2FalhbPoxZKtEaFAdIjaD42juDjOXY6D%2BTdHGfIRZZJ%2Bh%2FYmAaUAzzxLUcM%2BHNpM8%2F9WkJ20ntMDTOgWL9jd00uPJxuGreOQJZ4LqMweXzAp%2Fh0bRO6Wcx19Odpd0UwCaZuQUKEBP4T6pVfsZnERrSFe04RR14iL92DD&X-Amz-Signature=2f874d337ad1562750b1234441f0015c4183e3281c66211bd986b2040154b6b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

