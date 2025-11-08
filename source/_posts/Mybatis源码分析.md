---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646O6XEJ5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCIBzisrua47pFuS9kSuOU0byGeGD9GYmRd20MRMtJsISiAiBceDLbRaM7VU735lTxlcVIocJmsFmbcpjwCHnDV0IGAyqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3tUvf3QysK15CnDCKtwDIjKg6Wo%2FDs%2FYSmHb9zpx2tgF04wlM8EGJDiUEkL4xOVoSdw2khiMR%2B%2FA3%2BLlh19rutL9yYHMCs%2BQ2oQ%2Ffj4ur5DWk8dawgtmFH%2FKk8qdVQXuZQUKPxb%2FYCV446Ihq8WuvO8fq90D8VysY7LA%2Bwo03WWEVjJeEA6m3xXWLMNo00RgS8mTwByGnOENJ%2B3k9NpfuvQsj3Z0%2BeAYx1K6qS8%2Bo04PaFpW7gBX8sDRvmr5bV3XZl5whGBQdJF3Bew2SrMaPgJvmkHA2TwRRFsml2d3eA2fA2nMfC4MhRgUA9PYBMqlwNBDVcfTCEK5wlZd71tTCd7EZAW8kjD2SqD6j0vk89Zd3fb1NRa1FM6b9GloJJQSRspw5qJskx4%2BF%2Fu9R6wht8GRfMSaCAlDIJGCGVf9jAquFFXFvU0kgoEiXGzbEhmKxDTU8zEsq5fxpNBbQHHGYilRqI5BSZraOYwJ3Rd4uArPnY7OhMOD19cARW4zaNG5imU1XwFBiZQOVQm85u8U%2FRLYXyFPokTOrdwgFqoy7nFd0grz98zCu77TXxmYNk23vGl8SG6DM942m2qYN6gp1G4wQq6yU%2FEDuTFxiXlwGXTdKYMIeRFxdbSzfZAv3jFqDJK1QdBe%2BbG2VxswuP6%2ByAY6pgFHkwgQbxoN4NAK9fctXcqnrtuhBY0dpXfojtvD4vBITB3WgkIpp9M2EVaTZRQakqDqwUK1nAYPh8JQYDjYA3aPugg%2Bdwsj3PwoZUI0IvFES%2BX5daHLyEtKQGkivKHRzu25AfnwyRJIfdaYIBRS4O0zTbS5sbhaKNQUPv61O5vXbSymY1td5EuQN9CvSjjz55tBPzse%2Bv%2BeMnmN%2BjUQIU3a7iuOOnMm&X-Amz-Signature=f7dfacac27abc28c940becfc7ecc6ebd9ac56808b5766b9e67061cbaee1bf28c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

