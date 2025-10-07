---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUS3WBG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCIGHviEQsEu3xdD56ZD4oF45UCP7W147uowOSXJf%2BecIpAiAIBLot3NxQzmLfbdcqszNaUimEmZUe5Tn738u61mjaiyqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWEL7PkCy3eNkCWN0KtwD3LVYE0S1ZgdF8W2CmjYJQA1lRqX%2BHRx6p4C9Je4iAwaS2Y875N6aoUxcMkAVnx4i%2FxxmLdeZzsdjaqwFI6btJ7XwTOIUlRTG4QGMdchkagieAerVI9vFZ%2FLBmd%2ByD7j2G3oZPeyjEE%2BwGtbXD0HmcOuIdCq8V1hy0hTI8kBUXtqhegLzSoraBUZ81hCJvkGnKvj7FAjEzIqCbIDyM33vOh2iOsZLWE0ECu0IRi4O5upmNNL1HPNBYiL%2FuWLHjIvRQ6QoVK7BYLHB3NjE4mQ%2Fo0AflpQyuTpqwtw0Y4Oe71OGbHs6kld%2Fi6b53QZfvWmAR3fVtk86ELH09bhUE8BZfc4wkD6ZLYONCnOoM762BA50%2Fl%2FmsQ5sDVHFXNhFYnbLDp4Y774uIRF8Q%2B3Bl2sJbP4is5elLCPQzel2IY1J88%2FH6lFwzOPwIAteHiKl18rRpnKxIMgeit0tUULZwz9KLybf%2FQzGxcSi5167Nz9z%2F579glMtBiunoUTbC2LgmEoreEojzBZjQnzTLBXkXcbOg9QNf6LbWyo1DQ3YrZ8WhA5x96%2F3%2F9T4eQpEwepcLnERZcKEY0%2BXM3N7i66DvXAGXeF0ofW4qkwLJYZNTIePo%2FhBi4UKXBZWaLo5Z80w9rSRxwY6pgF4cCIMIzw5ke4fjOCQLTC5hLb%2FdEDlj4gQQ9CieO6PzzuceSKfeQqR9XvbNP2IL%2FvDL1EHCj0dLi1aQR2essSy3CA0hLgYKW168wjwYyAnwjQL%2FirnPQiwhKzxwDSvWvbURiQtBbXU7yuyRHDroXbNQGPx8axUXBlqEW8vreh6WRAqJuH6Wosr9wFM1%2Fcg8fanH%2F%2FZtNcxA1H5sllRgQnk7PsKkv3f&X-Amz-Signature=236d43735d6ae66155c8816f6de0155e11e244661d9161aef3da6896545a8ea1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

