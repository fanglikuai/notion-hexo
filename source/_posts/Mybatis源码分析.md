---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGXQ7MCD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAUX156tzQwJmL3lJQitOHepbRE%2B4HEHr2E%2FJZ%2Fe0b0%2FAiBHSbs%2FdNNjHG7Y9IXYiVnL3xhTx5q3ApdbZPlRqkhGkyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHyNCpJzM6mqZR1GsKtwDVa5BFK0AmPAQXYeQ4sENC2Dhl7DoxmLPZVyhhhYjMCr12dMMonh5YX10F77Hc%2Fn3hxVcurQLUfTsJoyBA%2Fa4%2ByQ3c8afVmuGYAyUkadf%2B3xKPRmeBhsstCk1ArDj3uoMgMiWx%2BShPjIh3nRE9%2FOwBaY5K4EYURzqLR5ZlrlpuYuceFS4y8ZGodHMLOORe%2FIhIFHCfAOLboi38CxX5eXgPdqEG3vY4FeOtpZSH7oeD0qyYiX4sfdTULi3szbat2HQ2qEMtPzhoo6a4LilN2a6W3qmz8d5eFMCS5HphXL7ewO0d0Do4vhneBMqEq4OfEF1Kgw%2F%2FOiCKd8CkyOO%2Bznsq%2BcidixuLVI8jCJgxGeePkTDCpwbxqIsX1YOVHMzwnDVruO5d%2FqdJUXa8y%2BgBxD1HzO6A88AkNrZqR5sgd5IulVmg7YkW3TMolcEjx4hVA2qy0SZmXEnmKXvUlfH33AdWdxUe%2FT%2BXORLMh0nIeY7Ii9yKv7Sxs2SvGPnmjUgiRuxxyLtz6RpPQDe%2Fr1tZ4Jau2i%2ByqtoPeaZxgcD9Ez52%2Bdd6pOfFh8qhi1uDK%2FWe8%2BS8Ebmsls3cDmnyKhOizfsrPR22syPTvmIDRy9uS5Lj2RPjwUtn6V8ETPkedkwpv7lyAY6pgFM7Q3VCNIUTFJD%2F3IZGeQy6Ueh9xS1%2BOTouyN3F1EqQHHeWvq0qcogh6A0fs%2BWC1CV1PV2JflKRJyR1V%2FfvsQS%2FbOiO8Oa5UMeyJcWbkNwO9JCV7dJgKCPhm5FB4eurk9CuZnmQkE1Rot%2BzVSQqvZQF8briXCguoFM0JpfHsofdnplsBWXzysvg%2F96rvJwZUyDyV%2FXm7YY4izmzVwCK46rs1JWJxlk&X-Amz-Signature=f3a621ddee545f4f7bac01d9d597067bf6e4c1a07f57d2707f694d1bb44e2642&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

