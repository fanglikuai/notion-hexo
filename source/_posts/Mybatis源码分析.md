---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664562JJEO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgp79sQIHKp9T8El4BnVQh4PxfK0OimXCbplFdBD79nAiEAkJmjVHjI30sLuP5NepCdT83BksXbIK9hg8042NucTgMq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDIAjZyDZh6aN7sUMlSrcAwA0NZaNpvLbiKzqGA%2FAL6V%2BMhq8Ngpvx97v2QgqpJA15zlW2XaPcV8qXW6STlAuqbwgrD0T4nzuAoBpI49pZko7xA1Vj9cAI2YBKoT6CHrB6f7XsbO05%2FatPqEhKxPPXkmIrdE1VVHuLsc%2BI0iePh%2Fig078VmAv%2Fz0qwTyDJX0oVhUe9Ax3uWMBL0FojgM57SA8%2F5o2SlcIMULR%2BE2vkklRg1GpWj7F40rbe73W%2F59fFZvI8X6QgixOWM1nCnXfm%2FRxl6dpoNQ1oDG5qoBuXVE1hmTP87sV9lasZ%2B%2B620q12gwMqa6jyXLxejNVajjSbLBvSw9X1uwijZxx3bG63IYaoJDWFaa4uYtbDN1e3QS3JuAZ9kOkZlVTL2yD1G5UIHn6hL8%2B9QRvCLxEdlz2DMIKup%2BqmDUPox8rcar4yCo9UkYw7USRSfzVL0TDsonsHtcbg2Pmx4DFiEnLkEBCMbjB%2B5FurwP9lIPu9CIzg95ThlFvzzdkvjNM5VrwAnpKH0%2FSNntzGrmX4ffa2jP4%2FPVsBYJR%2FtFQq4Hgw1QZW%2B3h8dL4%2Bbh%2B8WMoA5Hwm6Bea9SOTt%2Bv8TcIR26%2FvjJXGQa0gMf15xjtuvG7xyxKJhQ0RNEQjCnNQWw%2FkXMTMLaT%2B8YGOqUB%2FTDoZ6xltUaal%2BZB1DanxY1rvnIpwSwdy57VnDte%2FAlKYheVUGnQIZwujMeraV706dXHA0hIGbQsbWFrYHUQxlkcgRGbgmVZu%2BpxcVy89wAZBxlF9zFQL6dzFg9CACnLWzzx9Gz7yGBzbei3mzxCElYgXlX881o4NXo0Q15JLmKUovIer2DRRdzTRfLg2c2BBlss9b%2FRoKzsIupjpoPGFkH3qEO9&X-Amz-Signature=897629339bfd19cf4805e22b7046d6b268070ac1c3bbc9cd1f6ecd82f1615e6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

