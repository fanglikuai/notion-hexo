---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OUZWBMK%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICriY0rbtKxyZ1NQ29li6XiUPkqcmQ2PP0mtJAohNEQxAiEAgRy6IqPPr4zlgx0P1czxxXq1WjgytL%2FyMqqstxQyskUqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDW5xxxzCxz30Zt6GyrcA1LSulyZdw2feCyGtU2OD1wr6OhZFeLaXwTOeu4qaEB7%2FG%2Fxz0apfDDsd0we8xV1OhgKABnnbE2YVuTIrVeudvs6J%2BbM5XytMGynsY7wk8Eo6AXzgr%2B2uRvjUSIHGi9P0GBL8Cbtggi4zrHPgSKZgd6uYiYQes4z2Sc5MPqsF3c1bz1pd69LYDUO7f0rtULf6jGQ%2BQz2fPu9exiU%2F1DCkLGTfV0nKZZcM1AfUwodGZYXhTweU%2BJucNCsrG1d7gv2h1WENkzxAXUZI1PGfAbu9nqE9VPCApRhV9qitOFQPXu%2BdvzwYMBp1xWgKODoJftkKLNH%2F7Q2gDpeDElo6NTtUDDEodqkOFWxDMly7EW9kBUri0kG0%2FBv6NlptDP8QlY4HJNsdrKz2RmuKGpi1cxcP4pnSlE3%2B6aOb%2BpM8ZLNn%2BptwtOpG0tcxx%2BAqfkeDJVOET%2F3uaN%2B7YPGvvM0jcf7CjnWrRf9%2FCjqIidNy6OsSLYJgYjnJg0cUxTMECbo5ZE0wY1er9oDTad6ACGe8mqJRBWVKoRM7QEjBg93gJU1aDqXDb0N3VAjtwRtRcdiDhBaMbJ4OvrmpVTN7rKiJXRfIbmTG77wbDxs9VsLhL7n4e%2FLPxGNCcKL8V3k0BhtMITC48gGOqUB42mEvwolD7e0owwqmnFVLyskrFroAEqWCYogKteuQuRIADSi9nGoVc9%2BQZpNLCRQhkCPUU5eZ69UoPctbaCk6%2FfNYBz4CJSciSzlZg%2B%2FW8wNZxp%2BlQx4MUlzlQ6kkAQQsUbu5bH%2BcAuw9F0lc4wlzZcXvBca%2BuA5wDGjqoqTWbkxtFscrjTcI2scFUEvJ1AXVNjwU%2BPiGRr7RQAa3R9IQoFV7zzU&X-Amz-Signature=fddebefddcabcb1479883ae80d7b6cce966c2fa8de32e304807fdd3c20e9cc6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

