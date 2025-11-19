---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCRFLFO%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBRIIQN6r%2FSqng%2BbQU7F1XWnpdNTgki7w5SmUZ0fkfLBAiEApSEbwK99x8TkDi%2BjGI53apkmrMed782EaQKTzMUzYNcqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPO6SBBIKl3luQs3WircA3%2BCRJzZ%2F8Ta21EFWFPieJV0jlR47fvQzh9iY5kxDNM5i548O0EerwizVuLrpohiU3miwebhh258u21nKi0rtDWYLov4EXMAMxL0KYY7edL4wclBCjQPS0lZ6Bmh8Dkwy7gcyVmTS%2B8LY88dNHPMPeubNW0%2BlZhJwAiXoV%2BX%2B2iM%2Fl8twynfP%2BtsJ4P%2BddugwngwCyqLW9G%2BMZKB30hzrALIaaBT76aav4wooDe2IiePHepK66FzQvvSPt67zF8ilUg022X16Tlb6c5Vpj2vFsZFYNy%2BJ4wvdiW7A%2B1au%2Fj5JJPqJyFWLcLLtzjhSLF7c%2BBzljNOuC1lq3y9WvMhcqL45r0OfOwKlCY62nAhHItjThpXwIjhRWl%2BjTD8AzX7Utkx2FtcuSfzagakRU5Krm1nCxrqF1907n9eaIrBxg6XQHN82pGL7lf%2Fn1Sg%2FpT2qaBXPQrelrj4JVcKS9LAbPBmAJ6YKijIOmr51HKbghZCQDf7NQzReiGBa3zEV7yB6uOo%2B5rTJE4uaGnyRX%2FIS%2FMB6GbC%2BrIuejKSmRX0ODDlrjIh2%2By1O80tNvwjulR08MMKCqBwh6dBZLbgKY4iIPDPOwVKOiThnxPD6IqJbq1bEa%2BtL%2Fw%2BYewxoXzTMOqU98gGOqUBx6v4OLFHWHiTwsH3mwDjKJkkFQchRzzGP5pDoixg12SJkF1ZdXaIhRPVY6tRBU9TLUn7d%2Boh6C2i9cylzrjzUIYLIIIddiLR2eNxKy8p%2BuqYnKMwJryI44IalXqWjsrE58ysPa5LShhUl6C3dhxM%2B%2Ba0NjTW8XPVJBeK8qHF0srN8CMPjxSqMSf%2FdYzWDIJE3lMWJh8uOBsqBxtB9AOBEa91IL5I&X-Amz-Signature=60840db9adea471601608b284f4c2e3f9cf7b8214d2588e186eb4ee6025d3898&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

