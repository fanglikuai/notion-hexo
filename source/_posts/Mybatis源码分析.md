---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVLHUW6C%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T190100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQDIHMsplWinLve56JTLiJC6LKP%2FtnjUJEqFs9KnujzEIAIgNIElJJvaePywQSGRsDNb5dOaJ%2BI4AJjPzFWeQgwzUvoqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW9cPhXP5GA5b2u%2FyrcA5S4wZ1IoQoa1WeljrxO5J8%2BXgv1eVeS1jgcywwokqrj4b5cTsOo9XMOBr4YwPilEdRC5m0QuQsJqv%2B5WuR5kbFOaeO4aVNB3b%2FWtyitggZADCamYuEL%2B1XeK1VxhtYOGRTP5teUi88VGkkaCN1zAFXjd%2BA1YOoqRLJfiE8Cs9rUVv6WVLsjPL6M%2Bzu3dsfThhRSEo6%2FcEEBUVw6hIDbZoqB8t6nnHhEljau6oeNWZ9Qo4ggWhuowU8lCaUH5Cn%2FBu7FHKSwk9odbwV%2BVJK0h55IrTtLBBoeiVi0xUY4let25Um2ENxxq6tGl8%2BRHFO7VK6zNj24hqLY0ZAd0Gi2OEZu4EQNdfTgIugr1BinJTiMpMYjO3zNWsxdiLdF8wSFJTHIpsqL6VwZQVC3M4ksZc080lslNvy2tFMFs5TCc%2BL%2BoXfNDFO5DX2Xa4qW98rLgqK74%2B8yaS%2Ffor2K7pejX650NUcXnKTbMsuvpE2cGFX1CgrOpZniC4q97JuCkopaP9CsjeMNgoKsRoxukiiVvA96ploQY3uMkRuYYbKqmo5L3Khhk4GOk9XWhoTdTlB9Xreh3JFoqLcJ2nALGUX2wdLp53raYa0QuQlHfxi0xJgb8fvnXJhQc%2F9eiyNUMNe38MYGOqUBZhkezkw1VB9mM5uXbeBeNXOmRO8W7TRFB6mOzfx9UiJyxc3Uh3hGAP%2FyuDdfIUVwattbYl2sIK0M1nEDwwpes%2FaXtmwE491jtHjt4erfQnM54e5APFmFZDJTJy%2BCDIlYMHofT4O4bAFYIVY8RmW1qkuI%2BWQOtarlYxiN8amcHBpSg74Pt6WPjExfGSYg1PqTdgIDZ9AA32u42UTQEqhZw7QDo05w&X-Amz-Signature=5d05098fa5dfe71c34a9a02828bddbe7e886e5f023123d6eee16adcdfa53acf1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

