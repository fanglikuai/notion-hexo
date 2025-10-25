---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FSQHBU2%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGJ1z9EWqxcGHLLGBqDhLUxLMXrMiwIpxqRfgLc%2BsvMxAiBjiVjQWzzRkMOS5C5ZzKqJOYtfzjDd8PWYs9qQxr1YQyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMNexi1HtTlhGuaPOFKtwDNJYwk8VrruTL4%2FJR9MFm%2FLBp4TrXhmJFQdo71TjyKw0P2AXx9Uvr%2BKkJo0JemqJtYV6QZHL4NzMMY9aP20vICThICCldvL3idPrINjrJsEQfw%2B1DhR7ZmruWyDjMD51ydIFXV%2Fzz9GPtgBsV6KYNcr%2BLWbRYUg3c4%2F4rGzLUUpZfGhgthfIqwiCU37%2F0yRBZS54ZKNEN9VLPpUrJX2oJpWyC1ouVU%2FeLgS70mH2uwO8A0dq1hVPQRK2bZYSb2E0Ab%2FqvK6P7CWMGS4QJgydtQUfV5Wr8Nxy9PN72WVphYppnjMnU0zS4Mh7n51vnmIJpJ5dqnqANZ2wZ49wTeSY9TMxWTMc%2F3yZ2QjqRLmaTnNVX1AqHnwF6YarMb3raaB4sk8z8TQqC3kNmFecxEIFOBUJkNEEb7fK4zGO6UrvlykFMP3LjUQpP8EcYqk82YdtGPNxwzdC0DWeUgBknODtNzj%2FKqt2di18AzVzs8ZQ71dHTg24%2Fy2b1b32TMhA3cPklq6mK5pRMsh6gsIvjEPwJ%2FkT7szFXMs8FsViSMwdjYUqh6TXnWEhGxPWmrFUr%2BvPhMNtqwiEoSbUWIqi4J%2F%2BP%2BTRkqrhwHAPurYeJK1ITXM1gWUQqpA4mDMNCQi0wjf7zxwY6pgGMIJ3N%2Bt1m1%2FpkUmXpx3zeivrAa3BYgZuLBv7TvHKpZuTJoDI0CXwhNu6lGb9IIro0tAimRtbAPTKm7DeppnJw17uXfFAM9jQDCRJGljNm6vx7PlHTZMh2GrYb2OK5YUx23jm2WZ6c0CvH%2BQShxMDchO%2Fg%2FYghoq96%2F%2B%2Fb3LZ%2F%2FWYDcE5SEr3XNkK81BQHARQR0ksDyHLwkzbEJDyTiS867tB2Bv1%2B&X-Amz-Signature=4407eddae44b8512ec5b0d05b3b6595cbb5688cc438bbaabf3320c724a37289a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

