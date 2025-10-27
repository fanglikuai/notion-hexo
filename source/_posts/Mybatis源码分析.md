---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YEFRI5X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGbmmsLvEzvsKsviuV12PYqTZ33CcOoAHoZn%2BYFyTFxfAiBmBoYFyEYDdEe%2BqVzDqtzbgoVP4WZRvVC13Xske3xBgyqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTryColROW7A9TPT9KtwD6VLRFD92nJa8H%2F8zkfL%2Fu6j3k03Ej4zLYCP1T9J4U5ia3ACryVOqeRHZms8p32Kc8otjWJQ63H4K6sItDX7PSXRx5DAUHXnhr9uE%2BUyq6tr7q3gmMGeJaXuMJxdgUZKrs7s7M%2FS2j6hSRx2hkliuUQWYAhK%2BriHjPsmBP64S72SBQdtU4iWugNFPW9q6XgCJ2Wt7MvXixJCJ6SVoqyWqqlzw%2FHKkwPDCxVIkmwY%2F7K6CxLwnDdHG3PQ5jcZzK61riBJ3JgpMu9ZvtCw145rIBE0GGYp1iS1X9QsJ%2Fu%2B0ZPbvkSRXgeetzjVwbcF6LPLB8%2FWJeyp%2ByhDjvqsh7N%2B9xu8ugf%2B3375W4vbJyEOOWemBQeuNajFVQvaawkY0UoY5z7LZq6FnIlHV4Kc%2BwbmXwD%2FFKbfNCDK8k2ush1Hy4zX9lb4yWN%2F%2BsCAgwgBLhxkEb%2BQKsMqPsGO0RB5%2F%2BUnGYdTyo6YIxwKp7HdrVUisD8SQTK2iVLB1qrr3Pb1msVeyOFP5OUNGOMxIqDBrrfINvI6lq4e7brdwuRyE2oB2T%2BzEJP85lCm4o4qrkSFOZ7YPU6mycRJtd9dHyg%2Bws7PjAvLomSMosxoDJmPmce7Zg7ZRSuxH0T4YyS3mr3Ywmvn6xwY6pgHocZPCEarwd3hVPHdLxoLk7ufRj7cVOAI2iqHNkW947rgfqqmI%2BxrlhlhmEdT1JfMe6ox8B2HJIQtnVk2OcgOKTTLKqzHhLFELhGHRp8jlUIug%2BgAe1k%2BtMVAcmDoKSUw5zgKy1KrNzLKioyH8Hc9MeZjGI4Pivgd8Hf77ZSPSc4HeN2FP5xKC7PjQ6Hp1P6sLDF6nl5vFdpceCpl9lXYXE7p5me1q&X-Amz-Signature=d16a1dede9172a358dc348451f43770c342ffcd22b8dcc329942dd988ba05210&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

