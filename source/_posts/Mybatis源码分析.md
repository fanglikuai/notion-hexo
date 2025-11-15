---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3J7NQX6%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUGI9eyciCX65Yo1hCDJcBq2FQ3txG53RgrVfBDA6a4gIhAJCE6dWtGX%2FuV4O1Lpu%2Bg63gMcHVp4adXCXOJCUx2gJhKogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzSo%2BM6imZgunhEvXoq3APezAYLfpRyirlQamFyLJkbWMKKih8EIiPzc42TxNB6IC2A1lMntK%2FG%2BuM0skzlzRQZP9%2BAWBZAntJ8BicR92n8vFJQOeQu0ve38ZbfH4t%2Bi7A6L2fJCRkRbcnqQv7jrrC3wbp1s6cu4wCpMvAZJGHOe%2Fm4swNNmPOOE1H57wS8uBiQJ4Av6o7O%2FFSGo0bP%2FXeGB1wkZCcN6JOe30clZI9pbRVAJIl1D5CIKAAYeEzTNPGxTMceq0CH4lQR9q1GTUvR8J8MRz9FNw4z%2B04QTBbVzhBrr8jkDDJ6uUqOPba2SVHoYv7QY%2BM3xqyIX8H59SzBvKY3iXKQGGVUK6WPd8aAzCM%2F8RVblpMDB%2B%2Bggr%2Bw2b%2BOY4hI8P9akuwRSs4W1%2B9X6sYlPUISiC2Lfsj1Izie%2BOKKtzepG9QqkSIUgnBP1%2FkTnck63SdalvPwFtTK2E%2Bkn3u%2BibsUm9K%2FCk0Lr%2BAIEKcAvl6L6uZfoY3O1qRR14qy1AJiwxk1cT5S1jPmIMqS5eHAwTTrHij%2B7od58lCFVOkAJ%2Fw62Qmi%2FBxnriDYVZwy8w8qid04va4l6LLsGaGe%2BkZdTdSsnx5yBuY%2BCqiJdQ%2BzgjBKPUd%2BJnyP2QAMpPeBN6IwJLrVOIroRTD8ouLIBjqkAYJsLkAywu0v81vf8V6RE8b1P3KnUmbMbx58ORvJqbIgjmGHE7d1TZI9s1KrdJra9JwqUS1CCHQncuVqPIqWAltRauh7bXrvzj3y84zE2WiNQ0fY9TYoxIKoO5jnd0QjonCDZwr6ft4dyh4UHxMgDz1Q3k1IeSTZSwztWVq5pGa6w0njAb9CXPXFMh%2B7LnN%2Fch9W8NWdjakgw9T6S%2Bd0Ql1to0UW&X-Amz-Signature=1552fe0aa66eabe0fc8756ac49ee55176e864ca905b57a65047bbba5793c608d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

