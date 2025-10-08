---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYQZT5LK%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQC8Qh1DzrCLf2V%2BZJI1LDCjFwdfTqGsjkZaRXZIJ6CU9wIhAI%2BQY4gTFJgzJ7MyrHOUhbH2voU4ft5eqRs8r6fCq7QPKogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz3lB8KEYhg0L7LqHsq3APORg%2BKN7Dt7GEC87BmQoISVmjMEFBhHz3KgkXiBYt8WRZNQNpJNGewmUsEPDkzB%2FQuQ6nPIpVE8wdLuGvmdg085J7ns4lmTCzO1zkA23HWWR2qxXEva2Kfyqpt5Iz6FUmlA0ScvHXB%2FnGeK%2BqUe4uKc1QKS4sPnFZ4mLeQ%2FCCbMPINHciu6P24mMobfyoY1tzagvc%2FZeMdohRn42b7jkc9VyimDorPj8GYDUoB4BV7pPyttfE0QTU%2Fd6nMY%2FEvAi2hVlHmrRsn0d49JVNfQh2tGk59SeiAxxxotFKLnF8hcNhsA7NV8QynI%2BIUnZfbZn2EBo9%2BmYfwnca5lMiB%2FKUj%2BWImnMR2Xpo8ddcmq56gaKKGLeG0bOfmDVNm3bhIOptMIiHeMfhfZ9x6m3N2jtCF20BvgyrKCy7s%2BhQb2pn5UA8jSQtgj0QCvCcDKXYR7AqrocI68jZOgcS5llhgwQ499pnahqtV9l71KvFF9s84a%2Bijf2SwuTwgV3FJBWsGGKOJAQS2gFPLzy9O9nDSAtVQ7nFRrxlpas30CI%2BsDoxQdknWw2QvcLmiAiVGvX4AwR75EdzO7yUAO%2FsET9dLPcBMu1U%2BwhAgTgc5Ge7eAiF%2FgP2pBV%2B714t6Cg5IjTDu0JbHBjqkASuNMr82j%2F9%2BjjabbLw7NE8e8KrckBRmrpL8tpTizhtrZFbomI2QZQdaDCIxodai6bHvJhINVCP05htVY1pvFSqT%2FNgndd102UlMpqxCGlbpeq1MHiNpd8WR%2FU4jv2kmJ07bCuYlt9rrbiuS%2Fvt7kbgvSNWbbGHXoeG4mEjxpPsIIU5AH9%2Fs0ft3T911r67DjHrtp82fNeDDJpuVRc6s5nnfbRKf&X-Amz-Signature=232856ea74db4c0d01a157ae28f149ef7265d07f6221c03cc3f4542dadce8a5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

