---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBJDZDNW%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFnW2yg19WMiMvSrBDJ9J3dBrdruNvimTpiutklEGSXHAiA3fbACvtk50vBLNQfGMuLWUi8QoUNgDok9eO3RhK98Yir%2FAwhKEAAaDDYzNzQyMzE4MzgwNSIMvXltWVjmodnbwjppKtwDt7NEbEmQ%2B3UeUOF9eM%2BBAuaMJ9H6aZDvFhJRs9ezVLZUzPv8ja4dvcz6NDISll5WibHGfaDfe3Mle8QxH80%2BbBfbtm%2F0Mra6xtJE2k7JhhhRXAHSgHUXMu1yQ7CXAO0aN9TkHE0lZ7Apxael6UmSVa0HDwQaUW5o3HqdgpI7ZKKBxB%2FfF4oVqBznn8weax%2FKT9DeW639lo7Mey6Biy%2BXEaDsVKMkFPtI3D%2FA0URc5NuMAZeyE60xUP%2BpQRQ7XdMJds1M4AZHGbJoBqJaewzvF8UCp4d2JZDEXG4jBauKYSxYiP%2F7ZnlhhcqYrZh1SVrrbVX%2FXA7iFpiM6rhgiau8dXk6pHUuQVVQK13aIE1ucj0NMplYFmYDoKNv%2FazpZPf6r%2FZK7fcuwrZ%2Bpj74WyhSGUeVGxOQZfsvYFBvHarCdjdWGBaswpxSc9OPra6pDK6VjDrJ8vlO6MTc4BrHBDAjzPqfkgOLyjrofNGWJUQCHx6uK6M5UNqXiCjMJOR3dGtNRHEo%2FoJhndOC5dTF03Nv%2Blo14geXLAb8yP1%2Fcm%2B8QvgfG%2FeZlWOcGQlN%2F7a0LU8o72cYYcIMFfp4hqzWw7sbybhNdnzdpE2Uv4%2Bbz6lmimtBH6pmFbdHT1ZqcHIw4KGeyAY6pgHUM0ggAL46QkixoUgAIakhFJsceIkTyDvs8Y8cjD7vvJzHqDqyJDupRgI5fq7XHHgxwdF1VKmhZ8vlaK5Dopbb0r3n8YFWNbUGIF1TK9kWCvmsz6RHi8IMUoxpXZS5XV3cLsqHGImI%2FJjO%2BfdFmM6WQkJJLGjCfzy3ORdKvTbWWQYLorD9uMrMpqitfKItFcDOqPmaaG6BiY7hCYB4bmTq6JgkWVzC&X-Amz-Signature=ea3095bfe85d70545d053d3f921e9d137c78891015584eabec992b85521849f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

