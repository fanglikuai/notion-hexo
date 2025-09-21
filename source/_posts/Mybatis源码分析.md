---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ANXRON4%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCuXfc3%2F3Aar%2FtoT9nIhpJElIF8JpKOcDhHVEyB%2FHwrUQIgZFtWA9Ix06ajo9Jdz5AHLDJbliwdbRMa%2B%2FKjknr4G6UqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC9lFG9waWlwksVxUSrcAwhQJr%2BV%2BzY7b73vfO82FZ4RgvcW7fZJIZ%2B%2BjpaaluxNTbgqvXHdEH4waNK0baB4vJuSGR3oOji3w13%2BM26mOkJ9GoZkUtNDCcjKujDRFBAQalYLhnw7ESgmTZGGoAioiAeiIT%2FJdPKCUfqgoxTdSV9Buh6qqqYAQZ5jcnowztVsW7tl2I3Zu6FmMHT7gvO2V1sHO6RFeF5n3saUG75fbY6LGrfPbpBtRJQ9YMqQuU1wxMpSNa4AIfBmJxtOIfjUTt6CqJyRXPl%2BLhNtYifgMtSkY2z49XgHEc8z2qDJSyzSd4TBWuqQu2hiZVQx1L0e5aoSdwghzxXjlEqWXmj%2FNfxA4rhyKknmpCjquAWTs3R1aIui%2Bu5yvENh74bTDNRIl8a1FiWega5wuuc0OjweLjoEa1cYL0c02paL9NgvNEuRD1B%2BNpGZAXydNN8NOzEdKNa202HlKS33i4NjwAo8W3J7VdQeWzvaGnrQtuyP4c%2FwTonbFYnJjkeXYecg4%2FEd4%2F52llG6wdzEiYaP9jsqeO9fm0dKLt%2BRmgKJSAhOsiJGYfFk%2B1Euj1p4W3RUddlBnqUoxb1VEnaj%2FZu5mmSVgYMSmnWnOBSYtDi%2F3kwru%2FGxUtK%2BZWH1rvvBvaMtMMz%2BvcYGOqUBqWBcPHGmdbHsEkBchDqTLEpMPfUDLxe52Be27uv3m1vKa5HSBvJXDUZ7GbJ7OKyB%2BShl3qE%2By3VAJYRXLQtHNDn%2BSuairgJc5nNBfF712%2Fn1B73b37gUAriSgBLOlEQKSgX38ErD2D1ufBNRq5Lx7ugMPAs8Hn7gjoIX7Md1A2pZjq%2F6nNckgFoj93tGT0a5beYEEnNMNST%2BW7oc9DSV1pXJCw94&X-Amz-Signature=6b7a4f100956173a15fc63f94ab9f2da10815a80f8a80ade34394b486c7bcb3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

