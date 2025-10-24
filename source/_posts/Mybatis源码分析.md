---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEZEXL2H%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVx%2Bjf1%2BF4U%2FUs9BnH9MeVzoD67mS8iy30gGHv2i%2Bv4AiEA%2FYa8lxPSC2QDRg%2FX0a7eBflQBksh4LencqomoghfqSIq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRNxxdefUAtN0AjFSrcAy1m9FwZXzqxj66vJ%2FPEzJesvNv3GdR6l1XjrOPZK4%2BRH%2FYJeQeAaMchBeUAt64AEzQoGvHfjNXV7y6JaswjhRQRWONd%2FAvOdqh4rbN4VbDE5N8YhO42O6BdtlcvY0yBk3bmitKVtTpO20Jo%2FItjWDpFjnlYsnR9PTVAxEH3v5cty0K6AX6kE2Ecu5st%2FIGDsqLmpP6lhjpG8qiQmfRpJOOlSgUBy6deFSx%2F7f5Trh8G5WPg3t%2FNfUMYsTsVTkq2%2FGPfpseHBbhBqNuY8ROg0YWo0nG93S2TcX1AtrNHaEprzYio%2B2JGSsHCYxrXhu5n9FnCy%2B6fL7qSdPGuRKwawi99zCRpquHqfhTsCbgtavEXld8iMFAjgbZ2DSFLIE8OdgF9FvuwPbCUjtUKukBuYpHJd%2FfwIWfUvvUMlZtgd4iXJ0y0DGcOjVw6w3981bplo6yVZLZRFtZnwIUAqxgJMeTYhoU22QAf2iqCsSDuNnz8q%2B3eviBer9AS%2BfvhsAlRRJvb8WYak3xSKaTJg9%2BBQhplSbf6qBYJUmEskcrBdQMwq7KjDBlYGejzEYWMQMNTiTqwEfhJfm7b0pjYPmlFLJ5mcA2tG%2F7DUbaa3vEGkWtcxDNXNIdmf0nvxvYnMMqu7scGOqUBoBq%2FDX8MdL70s8g3RkVxcOsw5Z3PR4xw1hnhOTzzJrEVsHWX8uQgJWHqjlomfu7tWdrfFNG11f1be65h8QBW9DZmSnmW6AbkdU9l5n34r8LLQxL9qvrdy8VeNNLR3kOSUc%2BinlA7o09CpcnOXnxbe4vcc6UaMzE1ErxhRBBwm4vWZ24k0dsWMtbLAWqYJH%2BDP3ReaD6Aa0fVtrUGki3XzxVuHzaH&X-Amz-Signature=cf5ad9b8dd46b414843c601753682fd53f65f620d52d8b18f9c0512a5d7c80ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

