---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHNLD5MG%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCDf9DXVXm0Nis2IM%2FU%2BPwanCOJ8b1iOPoMWXv%2Fbpz5xAIga2QizYqBuf5XyZE%2FeVLsPIOPTorr%2FfZS36Ql%2BIS68ukqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQYouPbjm21SP0wAircA388%2FSSLxTyhyv4aLrnwnCW9QL1BaTBY1W%2F%2F4ZVZ8OBEb17yO%2FJzCk7zrmNka5XRacht9YlHNdm2RrfPbYw7YwjlsSFZ0%2FzfUv1agQK0rUmf42mseyhoygS4gdgsnOGLn56mqYTfRSV3zrPLg%2BGlyCclKgjP2RSGRfrz5AYIkvti0qvfmGAo5oMyhuhTmnXfZDO7Cu4eefE4ftg1olWhOmXF0V%2FMuKgtjdp0O9WqYRk%2Bdfx1hqDzXZfnTN9Jw9Ra4KvAZmGBl9re%2FZT2ZhgBC2hwE8ZgvSGu5kD8QxWlKLxKOb1SDCIPMF%2BM5%2Br20Rl2efaab1c18ufuf3HvI1bI7%2Bi3tSIT%2FCWnk68AGZHkHRUS7S2JSeH7SQpM9JLUDFv64Ir3OczU%2BcKB1FbCLPy4hiCiF0ty5rPBCmLF%2FwyHxdK1g5RXjh2hpSRkMHpLua2TQlJqbhM11IrIfhANvqW4U7ZOalSLQZLBEgWqt28bvzZRGypY7TAPOS49ofVuHm0pEAb8z%2FvwlzMx1RL4AVk7X8fW6kPGdY8x9eEzbw2jfpspMUr4ImbhL0YpRsFuiP5ynXxpmvGZ%2BJJ5xFxRvS2%2BSwnIoWpKavLvwpNdOwGUQaZ1UDjFeMwiNEA830hmMP6k8cgGOqUBnm0Ku87FWNDhK3CGZ3DpJxVxLqtk4zRueKziQOJWy%2FbeN265P9IOES8iTbCEkovH0426EUwFiAImfnI0UNDpiScBormu4ZnLzB6CFrotSx2upCN0LV%2FXdrqPHhHaBn%2BJY9DtvQl1Wyj3hoSDaNf%2FM2kS79LTdULd5rnU0J7sGQRH%2FPjQ6c3zq5%2BgQ%2FCXXqWYvhMuq4Ibva6IcytzuEebnHLC8oyy&X-Amz-Signature=2e349f0eed09742fd45a53cce27aebe27b7afcc5cd3ff900e61f714aac6f1ce8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

