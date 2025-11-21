---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7F6J5M7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCsi6MSmeeFLde0xIA6JvtPBG7PNI6K8r3f14kzsutEHwIgRejugzFGrfvTBWkb5tBwJ%2BDky%2FyBf1mwNIddIUutCMwq%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDMkPi7zLzf4Mx0b4KCrcAxPry%2FAf27DtWHk6KuuErOyVOHPDqnRU7dAWT1mjgIg11NehaBkp0bqb6jwfYmZUvwDwEb8gvME4Kz1BTXiqK0ISQbys%2FjsYiNyrY4qExZ5Wri4kY%2F5W6hnyvpSCesy9msYhPSv6100xM17HJI3rAvVXr8SkaH%2BzrFLUR%2F76TdDII8M5zzMHk1hMkEEiP1WsCN%2BESftagTQHzg0LLmbjsk%2Fs2WhaGZiOTgXrgdSEPgUJWmSptcOkbJ77X6JF140WgyrszQ6Y%2BCZiPSv8xr36ojvLAAycQxZWoGuHY6jzUZy%2BLTAXZHNCsWKdJOtgwpx5dEFIXXT6Rxdwt68BfWrhw%2BmEIeOIpEa0kbRFAbJx%2Fyl%2F0uY2aM2Bw2I04tf4fOEFTPZOt0CkALofL%2B6OFnw0O%2ByjFIOvFAhChI0Ra%2Ft32G%2FhtuwUCmlWoKN32vDEoPb600uo16gRUnbs3wIGi3gQYSgBCRwEaWaNe2eF2dz19l27hfjwa%2BmAW04dFkwbNM9s01NizQH%2Ff7q9ulBA9JKTNi18z7DYzXpWlWctLz7mzmawbqe4AO5XIOIuEs5cz3VE5aja4UgMq1wRmb8E9XDvhbsibkCrIt1JzYFxwUY1zFZvb9LIh4Jbzd8mE3iwMJj3gMkGOqUBPwAYPxtXsgyMGuQtWNnr4KxMxfpXmxCCry4yw80Xhrq5GOxohjTStT6YJS%2B8ePLvJw5vI7BBD0WrfuOjK0qf6%2FQSuhK8U7iV%2FWetmdLNJPT2WmYiO0ZR11lEw%2FvhXheI1M14nmKirNJ0so7MH5kgx3qCUc%2FMy6ZgtKmE9sMCu9bnbyjPS1OxEJwP1EyBJqZ0sub3d5Sndw1oH1z3ef8EnVrkZMtp&X-Amz-Signature=d2f4522b5322447a64cbc8ed120927c91b76b85d827e101add7a88da299f1c8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

