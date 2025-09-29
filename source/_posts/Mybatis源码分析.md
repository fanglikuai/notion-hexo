---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCBNXBMC%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQCUCe4mTsRGAOY%2F2LfgSaja73s1tZlgZdQJ8f8gxCh0WgIgBEQOc7Lh0Z3uUyrygONtqMGjHpNdFx5dJx%2BLA4qQIYQqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJeGDaSQEfR%2By8Lb5yrcA5yusF8nnw0Rp4h4ZXDkp0uJVsYksPyNeIjeX%2FEEvyQv1lKbL3MO%2FkhYrTHR6YFFuAShV1LB0FFmDq5IloGt2E6gfa8Dh95leWjpVGa8gKiMVg8P0ytXh1WI22MS32xWtiM%2Btup1d7wsdh3buGXVaNcK1kY4q02nsy9cgShqKMfekRz5uItYjfF9gxdOrFTsKZ0ldouW0vK3WRQTaOQ1OIJkhwTWPvJuDvwrOhMhWXn5UOsjqKBIym22Yb3n8XsZBo8OLFtdEPl8ONkECRapTCV%2BkFI0eyu1NlGdToUxHsEzS0%2FDy0%2BV7DmnphMKGJqQVk8IzggDYqJG0FcXLmDp5P53UpW7FK1dFSC5BsLcoyNpgbNs6TJu61YxkPP0A81R%2BTNP3jDUKgMxrEsLAgtPDCA9CR1Hxpsq0OLNnhZctCQkDWy6LIMpp7ZqnuBaZqKFm%2BYabluu1miy7qfqFVpKj%2Fvvm2D%2FEpDyDGSKutOI5M%2FbjrWCq8MD8JkPn%2FCifk2cnFM0SDpfxrrEEuoCPgFj043XOKYujcI9tg3eA3UFJFjbX9RBwIA11uWtPojgyDEZThGazp5jgv9uG68uEtDoCrnsfTCyFjoXLhBu4SPR0ePcK7EszgwxkQ5FfMGfMPzU6sYGOqUBwHkWzAtKiRp2rGhwgZaEYvZYnLe9fLvCuRNpr3PU%2FqNJc3JidGLfdFAC1MCm3NWAiSe8TjkwunxtJaeVCFN4wxQvzEpD2hAvQzWdOhXj0o%2BeDl9iIv9IR%2B4uiXGvtLoGZYP0Wi14zi3UzWhWk2NR%2Bk0ieTYhslWWmwKoqbgzxEL31Zp%2FqYeh2jdv%2B7M%2BIOJmGkGFvv36C37m43tEY7bEkCsDzl2L&X-Amz-Signature=a2c6f7bd9569a2259672cf5d03021a45330a3c99ce3cc88a8a4a68c44c185010&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

