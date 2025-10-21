---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622VF6CIO%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJIMEYCIQDL%2F83ZyTM3xH0ocglp9mDPKykOnL19%2BOKKcoaVe0vlvgIhAKbSCtg08kvgb%2F4oWUNSBHdydEM7l9YoR0%2BMfnecVp2xKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzBhZo6bDZpHWBLzcsq3APR1f2BhKsbogFfyluBHULkJZSLiCFrOtjHCvolKiu8hMLbdngcOZx%2F3eIpTXCWp07iDyJxZ%2BRp%2FoLOhzmY024B%2FzMiezlWRmbPxnYQq1F%2BWb9WWDB2TEvSVB2KL0Gw%2BcEitpiTICzCyq1SZMdndthB24H%2BSSsbnk4kDuQJymQ1orfn0cBFOAot2kJBF7MpxCjhY9KpSgUF9hHV8WXgV3KMnZWcBVp1nyFuDNPwiPVdIoaE92%2BV3UHgKEeZQqwYMwEo7VkeUpwS6%2FAVP5CYy52qWp78WN8oMKAcqZVoUX10UNmak%2FqV4zfc1bF6LBGJ7RBbqJoDrrnjcxtysL3MuDbTjq8q3mhQ4Rh7LxbkLJdkTKaigqXMTfg%2F2%2BS%2BkY7XOuUdFRH3XQweoIvM2tZ4SvyNwVnFmpxK4RPrtx3tulVOPBMd640E75Nun4cjMH0MVZtmb2xStBRFQ9sz%2By0HO3zdx0ng9%2FepBNuBR8qcfhJA9YLN9FsdPnQWpgr4CTg6f71wdEs3KAJsKjhKo%2FxYYVZil2KqN2lxFpcqpGnjEUcDpKKLribiXoNYWOaMMTEhJgUd27jZ%2FwOZAfn2t1oh2FoRqP0ks%2BmINLT1wQidB74wXbQidVtqyESeFGQaajCnutvHBjqkAbR7N%2FwsICLmFkn3jHmfnA4%2Fu1fxrqrLY8ePrHkCm5ztRNiVca3LW0vpKygHVVpchOD4oSE22GyIs4lq%2FBsWOgJFv1UI8jrroIAohWK6Q%2BQoFZzQEYkSORc%2FNFNHNNK%2FdL40DhaXaCVW3sZF%2BJnPDQYj0NBoc9D64cOgza3TToUM3izW%2F9ugRpQ%2FzzzvOGXr0B%2ByxnllEDhpqc%2B%2BUyQQ7c9KLim5&X-Amz-Signature=45678ce09bba6f1a52df977c78eebaf25327a3762494777f40886c8d7392d300&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

