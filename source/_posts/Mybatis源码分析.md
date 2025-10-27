---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UESUJ42O%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKZide%2FnRARIJz4MVmD5AKyyMdahIgIcE4GIzOg%2FiJrwIgLDGHdn0uXuu1gKQh4HBkvHvxQhfz2iRB8hRTdd9mXvkqiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOsy3hXQEzJY6F7myrcA8hk1rgJh%2FYX7nqD8aSa%2Fu4rrzOIPZxkjF%2BEHhH9P3zjBhv7%2B0eCwI4F56T%2BkhWX%2FGb9tyGpfXuioPOGPA4MOhXgNMeSfbYtYv2TJn724EmNOGP72ZMQoQqkv0UzhoE7tb%2B%2F1aug30cgLY5RxWOW%2FwxvAcNg3nO4VrraQB9DH2fvX%2FIp76LrVQmI7nOtZCV%2F7TIhlqXpdHpG3AbvhfYhrBZEmmwrs1KMRnYnxvJYyzpNVzvsVMhsOHHzd5XkXl2tGNRo9MrXz8WMc8%2BFTFO3Xg0ymi6E5ehahoyzXSDRBvBfzQbFN9x%2FfNe7FZS89RbI3JbQJ4254fXY3cKXspWtiVdTVA4BsKEH5XsoJgY%2BpkUkX%2Fzb62Zy6bUc3FysFWzogpommoruuDUyP7BKHcwq4NsVXoVyKBaTFhgfNrFlbwebYv9CZexmxa1KZ6kyGE8y9OLf9mhGqaCERlzlSK6kD7f2fHiIoMFoxppOM0Y%2B%2BjOva%2BsANdc2iOkQhk1ECK25e7Al0KVxUC%2FAJxq6sKLNx%2BRVotc23OE%2FFqcOTAs7UPyZdLaEnRTX5w5Uzl7UH84MvNKFg6ruMCNFr4%2B0rHqFI2NoqhS6VJoW%2Fs41TnRo7F14165%2FvlKn2g0zq8O2MLGi%2FscGOqUBlsl8zKPzFZcuDwJy1Po82CyDFFi0Qu8fN2pzVCBIdJ87Gluvk32XYZeEAnCT%2Fnfd%2FSV%2F2VxONMn3JKcj9V2rQqkrT9H0%2FjmS0Oi5BC3ffj6gnTf1SDDVn%2F9p3sGujjDSjlgPSHixyBlPjUlLWKjJwqDBr0Fk1%2FDMr4j7E%2BNRx0hr9rbLV6%2B0xnkYatwnEFk4p1Lzxfsn4S%2B9klXjv1%2B6HVF7a6yK&X-Amz-Signature=754d98f42d1c55ec64fd7020afbfb91446dd3390b64094b17c522a2274b59518&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

