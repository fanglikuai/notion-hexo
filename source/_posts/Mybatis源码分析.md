---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4UUP72M%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC73I9fZX2sheGlrPtPMCUlgMo6%2BYQ%2BW8qXKFjlGFMVjQIhAJpzG2zeIeVnktlTLjvm833cgDX2MPoJBK6PfjNihdVOKv8DCD4QABoMNjM3NDIzMTgzODA1IgwQLmCbXtnRpJfDXMQq3AM4%2B4V77jgTtnQvQZZbnuVC1HdA9lmMBzyZFKy1fhZitb4zbclsCc6uNyFk6X51cHF98tyEzTfhqEsBNsl2GZdZ5DT22HpmS5bjrCL8nAZFiqHkwtgxqdXv8c7ic2rcWWfrlwdoyS%2BhVHbgAGdosJ29wgxkDb4rslgWlkNtb61C6HPnIZlIH9dPNI7IIjNcfNHGDnteCln6qsNLgKkZOhpuLurqY6sqFYspo%2Fm5ajljn%2BJVSIq7nVKQQ7tebPQjLx4cl6IY4%2FrJ2lOSLnGfwdDHeyxHoQdaLtjvv537j4RXPfDpQop5NwrsPkw6Hfu3NOSSa2o9PAJw72IAxAFy8sFsREJ49tWBpmaXEQO3Tu1Z6Heewfu2%2F2ReP8KY62cgVr4pDkGjiRHD5R9OsjtSDMMLMn%2FoJgK5EjAl4SqMxv8ynQOWwxJu5wd8rI0zm9p9RC1DUNpU6FIQHbC44TKZ%2BcP9lKMXgMyzPk1Gy%2FGUkgwglLSY7z6YKKeXYFHMZKl1uoUcwIXPRNOucF7lcDSl0uVk2jZIVdrip6EQLBUpQxGTbND8rqN2XPRe9gyqUuHRKCi00QclKvrMUgLJcWfgPSJ8DJeljiPYOljgnx7WaoCltApB3U82K2RV8jnQGTDXq%2F3GBjqkAeTVtQSX7Rs0vKQzfDfWRwhYUCTN4suP4zzDEfijVxgbEcK6bQ9JrKXHEczVEtn2%2BJoeScw%2BCjl9619gxL8%2BpmS70VBs4aDF9sx0Hpq91kv0K4uPIgwy6FjG%2BmI1wEoyiK7MMxGo4oyitl7xLLZOVzfg%2BBLvaO4Fr8mJDMv7P963rlxKjh%2B5y11tRf1fVYqiBxHnag5j57onGBqRKkEty0KsBk%2Fg&X-Amz-Signature=b6ecc25533a033593482f93a94f9f18e48db2b2c18e42f9f1022c6ec80017011&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

