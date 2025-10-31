---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KF6JAR7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIHVvEMns%2BimOVd9Ov12nSmhK%2Bpdu1iEiR8lAPzlQoQcFAiEApIh5ruJSW%2BJ5ErF5sIOreGRsJdG7aPJu1h2JIP55tEIq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDFNqyuLbMdtRn4gVpSrcAwcmnraMoIbhZYwEQ6PrfSLItEUjEJA1o7%2FSbuss%2B26O0ZoBhEGsvvU5mm4CD%2FNrH0Q7iyXxLS4Oeyqd0qa4AC4kD46u%2FYDk%2FmhybyDx9eMBLhcnk8PWF%2B0%2BGoUCgFhrODNF4WsSi2Sd33%2BJlwOZwv7bneJTjJjqeJ6kkXoChquP0%2Bm%2BbBV2Gbeg9iAjgm8STdO76fs4NkjtMR7CtoRdcBMw5hoNzpITr28PU9%2Fyf5RaVBnmHOWdHXPN%2FurVfxEMzUb1KvKITlNdkbkaZ6XIuv6axsQgNxlfYNZo23WyNnmtqZrcqIZ%2FxQEtfCvw27lQ%2BbSeTPQEJL7HvGC6n6343WcWYn%2F8beRvTkLgV%2FC%2FCKnHOCLKS9jL7PYndBolErrJ%2FE6HWwuF6Am7L4AuQvMY7LW4EFWPEfLk1neJMePy3UWPDnryJ0MiZSphcK4InYAtpxH39nYjmxIz2uWwpRgXk%2Fvw3PZD4K24Uz5A43HBpRbNNMAch9szasDa0MDFKM99ZsMM1r%2FRuiisR9Fox4DHPzoYHHkaA958erpgIIIrvxrFx8txJvGkZEk0DcoFAaWNCib8zTigSEW7Be%2B1IVlmoNcJkOhLh1sJmzzyxAC5WmPJhuhtTYtGgxVjq7EnMKOilMgGOqUBohA0WEegpRFcSEJKp06%2B1z6Ps%2B%2BQGcDR%2Fe%2Bdpvji8AtzVUgpg%2BsPVM9z5WuR%2BxOu2nljBSsIw8Z%2BuJSSpb58xfvRK2Aqd1t2XyW2lz8OgElK19iWu5PhYSX09no9uaSe6EuozC1Lg8WF5ATGlSZyICZ9hTt%2BFeekfJmmQ3NEYEow%2FOr3h7%2BG9GDE4m9CBkiuH3QoTwGziWFLLWtFh8u9vHjWL2Ck&X-Amz-Signature=d77373ff1d8c1a08aa06e7b49f993fecf189f1e6435b97f5460a326996273531&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

