---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZHDGZVX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQC4faF7NLE55%2FuTd5mR%2FrVjciU7fqwP5%2BbwVhgR%2BpkuTQIhAJm8sjOi5uVft2o8v88BKfRfQOjXejKU3e7DqM0e9OYuKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2714cC6eaY%2FQqJo4q3AOCcfYpRdXV%2Fw7%2Bk%2F%2Bc0yKYfNh1Pw8O8HO7nurwvXimIN1n%2F%2BZAJa7%2BLQMExQLGDeqiry1XO14SYAoOOCtQcC8gPoVjeusKVamToGQNlHAn7W%2Fah%2BpGf14X726lME06wvMyczbppew5DbUGhEzauEgrsziiQ%2FgnBhxET6cR5ylgb99NuwzLkgOxdxCTeEBBCyFtM0lEwTxhKsaWTZD1NEOa8IMLODOMepjphxsvQYGng0DJHja08aIEVQ1SgIm5byz%2FPwWLJ5rloyy%2FKPZGfArfCuDnKy8Z32ZyEFvOx0j2c0aGxJL8%2F%2BMEAakMFUqU%2FGkgToJ7hr5eFeC%2FZQJEZXmPnmTeIY9YfUxI91FVGKU%2BvihHGASTobEaqBVycaM5Ix48ADt0WI0TeEfPeV6izocCqjytMc5gTSMbeftECLGgpRzq4mZ3V0fgFgzyZEKo41EgBOJNynsQJKlGi1ZfEUs9s0DNiEJsH5tPjoPtplSoDTJqTkvf%2FVQl5EOP6jSnYdkL2YEBqQDk8A0qRqQRyx4bjs7RrlFXw0mGc5B5mDm%2B3dm72GAAmR3zBpDS0sWZ6OkzQPBb%2FxJMFlgA%2B2YqusjfEWVThbMn8LtCgja6kwu5xfwhJ5CQvRRj3AS2qjDHxKfHBjqkAZ01VAQAZA6ozOOViBoighFBq9sn4u6K8PoGyQ53fT7ZcvdTjQ%2F1N3HgEj4JGFwYeSUpBiOjc7IJ9HbtMZb1AAnT9VzIQAF3CQGIzDDCLVFUYv8JBSCenbRH2BdIE4kFDtikeaeskL5ZxdW0eWn1pBCmO3wmAp0k6WIX7H3twPAsPtBx2DCbvOfmM9v%2BwXqDrRbTutg7mRkVtc4xwae72p5TfXHL&X-Amz-Signature=8f9e7c1a1e9f1d2d8ef0eba89e6c9046c6eeea99f4b8170049d062c830f6d029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

