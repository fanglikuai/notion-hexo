---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IEY2PKH%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFqP2L42%2BT9COB%2BT0b8pY%2BvEJsEkYTccOdO4rVWTqIg0AiAaTzyolR6r%2BBHQ3qZTV7N%2BTMFZPCg1djhs%2BYqBmw%2BPhCr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMyG6pDGX4a2hVh7KkKtwDGGzRDa98dJS7C%2B3AwOlfPF0iSsCjgbDi9m1KZHd2GSiBCGpkn0LPPwEs9QtO6nDz75Iv0%2B%2FzHjSkml%2FDphBJObTT8vBSNkZ1CY5qppvp74EBQJpLOFeGfPQ1j9KYVoQLbHYRW2uBnJwmYWDczn4QC1AQKsKUx9IqHoyMKS7Pl65JtjhGCUPXjzgQnplFrhTVbwG6sTnqwFE5plYZew%2FmJ4LTN7M7%2F4oYs8N2SaiiuX%2FeyijTplye7YozklZlBePew6wyaXC4ep%2FgJBvzMKp9sBcm2gHimXplcj%2FFWyINXiOsowO6Dg4lIvPIabaDdkaEUwmMnTt1J73D5lal%2FtcXn0Mh7Ps5SwydAmi0dz3txPGo4HypPDJceBucqUdcRPmvrwEAacEoRIktwzya6hbuIClsjyF9A3IHxCmBXbDnRD6tSYcSOXGo4g0qgvx0irg8BIYCQA3nNRuChBCtRcINMMAqGhB4oCOuKtizYY9Ig5naImLuYAMiMFBRpi4t2zRMAz1EmRzLPgAkqJ%2BzAzThM0oYNq5eAwx6WFJDS7i0ahpWIdzoWsQUNFdjel4E3OU7glu6KsdGB59z%2BqyYN0Rks%2Bpf3JAvYo8jEyalDreDdAbB8f5xjv9kPUQTlSswpNPeyAY6pgGggkYAxJx56QsD1SawE2JrRjENHDVbd9gBbJb2wS5N8%2F11PdlBaiRcNPU1OrP8E%2FMatges6AsMLYhDOZjqu7ccCUgtFkZuR3T6D%2FNnrRfEDkLLWae4p%2BhlOdzuqTxjrqn47KQehkSOjpD8f8OXcshJmbTFTjCR363opmYbFC3p7%2FdIBHQQrNq8r6z1P9dwyJ23o93bpFPsL7aPXiiqSNiBb3wh3CBs&X-Amz-Signature=ed571f8652ce1510ece684c6202a614e72e66fb34e0510218a6b4401a0834939&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

