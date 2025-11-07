---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6AZK3QN%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDge5mmOKqses75ygEDuZJ0jmvHhz%2BLufRn0aOv8cG%2BvwIhAKgg%2FpJAh3Ue%2Bk34FQbzkxniDn2h3XGqnuGTPvADK5F9KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx4idozHZSdzqFkus8q3AOXxtoGiqdsovaluFdDlxJ3%2BpcCkXlEVVK%2BgGvPftlXzmHlLwTaCzc5ENaPXGn4QOr7qgrX8rrv%2F%2BXF3Gq9zClVKsk7L8HV9Ft70osV5aOSET%2Fwuwdk59aFsTCwt3AlYaB3MbH3MJn3%2FB4T3HidhjuDXceQCUJuM%2FqqaLsp0t3WmuUCsxtJC7UE3YKDO8dksqvi1iqV01C4bdLy%2B4Ty9HH6khoUh%2FIPMMncwGKhWQGOjSJR7%2FZxK1Khg%2BI%2FRzjCHwhnXy8mGXILJYy1aLp7N3%2F%2BmO4ANApEIt4YRgVv7pMPpQd%2BiuRmmpsh%2FV1Dz2C7uR1K8qslUd0%2BfbLGlNzeR2GLDp%2BODKk1mugjaZvFJCP8m2OfVE4MsINEJKaOmDCP4A%2BdYJPtDGpIeDvx5QzpBRTvOs2a5cvaDushdW0bgdYMWY6A7yJgHu3%2B5DOQ9WAt95PHSiG18%2B0ZX1%2FpgmrqR0dePBO25ojKFudNe7xMwmEhtuAiz9I8w3ODhuN4g3R1P2fXgaPHuHouDsB4d094QEvlOzfVPu8fGmT1lgM1nKmTe3jWHcQt8D7hge50%2BTesf5dEwUJKI3RiSXPdaXMzTjWILScVobjMUmWRdx6xMRk%2BYCDBu%2FioqUSq%2F0xzFzDUsLfIBjqkAZmRDGo4CiPHqzdkDc3ljDoCdx4bjRKoLgE7mylX2YvVCE2DNBtl65noz7yhHMNOeC8uPxvH3eyvw9M2JxUvVq1RJ3JOFG%2B%2BKZVOAh%2BiYxsRoZVKDYCzJshWu0YmZid4RYrb6gyHDgqIa8iSWT0UP5sHv%2FIrB%2Byz%2F4oCE5FM28IQIqvK7Qw1%2BQAcWk8AuOxEC5r7%2B4ftEXYO%2B4uNLS4DSkGnMWwW&X-Amz-Signature=d8bbf082730f7dec5355cc2159be6ee2d0ff81fbf93a5200244388cd22f04278&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

