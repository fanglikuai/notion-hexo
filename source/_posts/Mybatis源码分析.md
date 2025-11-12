---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4EO6D6B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCIFSDKruO%2BT1jy%2BGp16ceEUnBvhF4g9ilM7fnXplG6%2FzWAiApHus5ocSAhGa5B6kOGb3w5IJ3eekWlj%2FFKjsbX29L%2Fir%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMkCKFYhlvz%2BIiKQenKtwDx9hN6kecMedU2KoP4gFrXy%2Fa57j3w8QvkTYc6UJEO5%2Fp09PSLhQoNdMmahJ9cOv5N0TEWmmIl49Wd9%2B7MO1XTLW4N8CsYvW7msH71wpIK6I9jzm%2FJKYLnABVQ5kIDQVA8p%2BQqQFjLnZhZiuRD3Ke9EqhxHQE9SFgjuu4R3j2KsoInJUOBE3kBkq2ETqjAxo%2F%2FYhWQZeWBjA4Plk3gdszEGiKS2nFCmxCbj8MW5AELB2bA6bbphUloHJWsc%2FfIYYlYPt01OpnYdqj9eNKyWfZsKS%2F4a6eGLxlH8g9C22z8s93g8kaRvnzgqovLhT2bwkt1eQ%2FtYoTKBW9OE2PKx5t%2FfIgKqEAWNr2TSgsmLbA1j9Rrl1LRqpurhFQ4gjHMA82%2BWJVC6Wg6J8CU1QJaoFtGLXBzyNigT4MYU98qE%2Bb3bhywCMl3D0EOv0LQ74%2BlSP55qL4eQQTk4RbB7hr5HcE8cNonZcrjxnyrzkkrhsPcBCGLnGLAAl8YDK12bxoCoZ1qqHPKeHJG5S%2BrDOso1n5xc0EYaDynp3%2BNN5zdHi77yCKQgrPPCmiJrV6hYfrjvUKXucP%2Fie%2FAWjKSCnPG%2F7lRT%2BCBzK7O2tzcjYHVQhdsMh8cq6OYv8i6KwJIxUw%2FPnPyAY6pgGILR61R6D4cuKnvUTK1T2%2FCjvnd%2BSJ8xwl2rBmaosowaJBW9IKmO2Fd25kyYCRMiE%2BZ4y%2FoRr8aPxI%2FhChR2%2Bor%2FzGYX09fKENKrQlsXEoikXT9VlpW20qghP9t35QkpPHa3aRswLjObHeop65s2zb2doxUCgd9A96hblUCPSTFmzMzvcBUrK3L7G3UFmbThzKxWi7dCHFMhnf17mQmamZn74Hyhc8&X-Amz-Signature=2eec387f9ded40d027b8e598e9256dd90c15c06de149e222602bba3adac39500&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

