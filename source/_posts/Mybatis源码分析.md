---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLJNU5LJ%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T140106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCL9M0a0MivLdK0E%2B4RKzSeqfFBAytGnGZR8a%2BDnMChPgIgdZYA4a2PQp30nkyhL7gzXHo5pVKOIu%2BcDAgqW8oii7sq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDINGdPwDM%2B7pXpcN6ircA5Nib5%2Fgnl64b02o8EsBFMIhik4gF%2BdZyKGVIy7mmef6jf6f6hHfDKOlbFigeivaGWB04h4%2FdKmGL%2BK38AkZAFBH06grcawwBW7W80j8aHVRseHYjFQ7ZOVd3fdY7OEDKnQErncs%2B7nbjXP0c4biQk1vOST32QaDVdYsbR2SNnev5oZ14kUvoDxiozXvDlnsHPXw%2BKerEa26U%2FONOsVQ20LsmTTft0CqMjWfPqMcB3XIFUIU2dJv6kXoRPzM6%2BXr5zKwFvnOEe1%2BqzbiHwGVh8utRVAgsjK0fiFoOcRYpW%2BX2%2FJWdpTqyXwDDPWShVQcP8yIcjrA0et7UBvVMrQjXc%2FPuGdzk86onm3e9b%2Bm9O6EnWRXPjn1Diizm1m5JUeg1OwqJ%2FCI5U4NsNUyN8kWJVrgI9A%2FU7V6h76WQr74uwh6AOQoSGVlVZ8RlNnAJNRneIEM%2FMrcPJikuR%2Fz%2BQ8iIXGjGAoaLpLm3SbtfApNWjMOtdRNRoJBhnGViRyse3Z1rpRJBTM79B76Cftdi9f22EgZkcBfzf7dppFD%2FNEvkZJafO6aBLVQwlLgnCSIuC7Ytb3PNDpVlDYfzuFVUkly%2BRnIEd4Cu%2FX9DIL9SecnZAlVftUeaQPMwaNB7A9VMKvZ6McGOqUBYc6mPsA7wJdmnTgnp%2BBhglU3K5wITqgxyzUagvUBl78%2FNrFVAQz%2BVevVtDXIXg%2BXSf4ahoY6nk1ZkdIVG6U5aNEVaVw07sIwMuSjaT43eQ1urnSfsKIQ8WruhpgIexsNCZIh3%2F5lycNrt8Sjtcno9fFoXblQ2hOroNQhj7QAcbCAtBDeiX7ELlE6XoqURSTgb9YBUA8eU75utZkz%2FFrDMP1A1iy2&X-Amz-Signature=61507d83dfef2a0b747db1559ae271e3e57cc667d248e8ba3fc9d6691d1b7e22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

