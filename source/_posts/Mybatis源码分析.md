---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSAJYO3Y%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDndZ4gttarasSnwswoQUVuh48eyykLynN45GYRTgSEsAIhAKWw1sXPFh2ErtHKQvrsDCoU2O1%2FGVRaPcSQDF4R5eN1Kv8DCEsQABoMNjM3NDIzMTgzODA1IgzAN%2FToGuWQSK2%2FZ20q3AN5YHCO%2BQY%2BouMdWmRDnoTBZa6RSVPAZJd5BTdD%2FqB%2B5DT%2FAlr0HcYGD%2Bmn2GevHKSZ09jVHf6Dz1RArXBZHugOZxmEBYv5PIjOxvK1zjKJfYBkqifmJH2k7sDj0IGLD0q9YBSK5tMA9yH67Wbfi4riF6PLn5HFXQqcKN4ARqga2FdFdNv3daSfZaY%2B019V%2FmMpr%2FzoxZofP62H5pP%2BQOi%2Ft4oiR%2B7u2GTyuoKdgXj8UL9%2FfxyupDJbOXxcKa4bREjeN%2BK7SkUn2AL%2FKAYjC9kVk5xAr3R97YmxtU04w1TTeZ9bEoTMP01C7ke%2Fri7r4LHek%2FtXzt01WOXIKRJvFpsd8mL%2Bqr5Nnfdh0rN1RxKUAcNbs2mmbLu4b%2FVFxNDAlyWJEPevdnJXKZCJYc5GVvKB8n4E831k0oaKCyAmJCEB2Lg9f0BZ3VwBakBWEK4t6npWudKF19j%2FVyAjKUp5wBm6UgD2L1mX7GknQuX0kdlhvf5d8OGx6F9kIPXwqHQzS5Mc4%2BWSk9S1RK%2FrpW95nPguxUYj4JBoG5KNblE%2FhYldK78f2nsFqDuUYRtQQbxBCrs7keLuECacqpYnuUDtZ09D7b%2F6PtWyIHPicpA%2BTX26d9Apk7pbo3aroCgCzjD58bTHBjqkATswyvbf6GZS%2FlzPhLRyRwxs7gHvA6fF%2FwSLHGgIGZOG0BXSugOXUxYAOJvGKQ5UN918BgRavNgyKCPatE8HDu5O91JZO3q4yOA%2FVG5bldyQ6zGtCGuSX%2F63Vrew6R2ErxuNUvovgRwc%2BN0edtp8tOp4VObl%2Bo%2BTb1hf9zd6wMzVhihVnX4MiMTk8cBLNIp3AJTtjJQO%2BhwXEkWs7mbt%2FPN33YZG&X-Amz-Signature=2231d2e74f228aad8da7adb18123097fec3d157fac15130c8dbe8a28e95ff270&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

