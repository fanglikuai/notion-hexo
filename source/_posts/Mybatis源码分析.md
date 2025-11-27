---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCLC3IIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCYbF3O8zicU8kicQ612PjeY1JGwxURSXT%2B%2B%2Bbphd8kagIgDhfhU%2FmFjgRyr44%2FoZ%2BzSqNaF5dMmI5ykk1SdIlUptUqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNDmuLROCAS6YFJsBSrcA%2FvSdw1GmcdyzK2kX0DJ9bi%2FMl6%2B%2B7yTZVudDFrFPfFy8bMXnDncFBpqFuXZKKtri5bJRqZ0Yqq8WrMb2c9hAldRbuiRkc62ykfpE4CBRJQfZt%2FEo0bTW0IthH27RErxLjJqVxNHvjWxKwRnJ0Rd3EvtXg5vvd51F3zLsP5%2F%2Fm7LmHxEqT3IM18DBPdQEekOPx6JJwOICRG9HK1RwX5qvS8DJoUM7dwkpeI0dTqAGIJRX3N7VVM18my4tvIEQxi2YwGJ5lWM%2BCTSBDEE1qSr20d8ZO52BwWCVDKtuJm7p1W4RfEofkXpQ%2F0Hn8wsJJkKOKno%2BqqtOjkjkk%2Bqyiou1rTXfADwgxD2xnCKDtkvmPFMqPLVw2Wql57uF2OT5W%2FNQ6b9l0XQGboAQKWC%2B0SuGYNUKXJQBCbBYQfChZXwP7oEfRYrRFbh6vr%2FglHYb74%2FvItC5CAahCKN9PguyY9KWW01YrR%2Bw7Irf2khHIxICwvuUhbnHGU5me2%2BaDp0klTrOig5VD4hdVgv%2FICHzmu1HrzYSb05h1M9ACbdpd3fiy0uwuBq7Wver%2FS4PqNGyDnJNHAaHvhMQnko%2Fa9OhnC8m06xxLcpD2a%2BpkgnR4NDYey0faVlBUJrcnMan2sKMM2koMkGOqUBxGy1UwcrVNDAHmZ2I6mxzO9CrfoIJxnTCRYBgD751DRKaubHRTymFgk2vSr%2BzHammKPSj2e9hXJsBOpgkh9tYlbbpZwg4PRGbFztET6B86z9FJ2Plyu0I5Y4hfgK2H%2BXPSUYZE4gl3yYJHN%2F5S93tX7r%2FTR0Doz5zjxgbmVNsB1kUU6BIiid77aV4p2LOjLY8%2FZTA2BvAQyRQpi78GwsC39fH7lS&X-Amz-Signature=a962278e5f091e69e2b2c70596bc488a4c417f3350bf2b250c216ca1ea4f6981&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

