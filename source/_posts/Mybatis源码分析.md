---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQQD6K37%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T160109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIB%2BZ8N8BQGMs%2BD6rUktrJ1ZcYOaJ09SAo%2Bn8%2BSaHW16kAiBXtU4lAQhCvqCvLBK5NEjQoLT2HNq%2BnqwkkqhIf%2BI8dSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1AmCIl47vybo3IzcKtwD1%2B1FdqOsF77DoEtR7jqB86wzvLtOXMyK0IBUwPyZug%2BxPdILEVXg72g7sWbkosYlWskOFFvImwaagTcuZgXfmlqWDwoaY%2BvL0hsI8NRk2leDks9Z%2FHTFJg69OB3Usj19M9DfUZKSy47PbkCvJ4ci2E8l2YIm1CFw0XQFVmg7rgSOqy2W5x0NYAbk%2FKDzt%2FunM4LLmqORjkSt0EA2kqKjg8nBErg%2B6%2FJ1ujHAcBmKgBgMVoydMXK31RqJrz1ypVxoPr6KjUd8YXCdX5Q5TE5Mz%2FyfL3pB5VQiR9UHAK7wtVKH5koE12lXrjStvuwFfWQZyTeD4gp77hxDgIztqxOBg%2FxHaDO%2B1IPKHUUPK0Q7wJEAqTRCRBMvQSbwIczB3Sf%2BhMdCLEuY553xHFFnehEWSCw2kpiwK0GzawIYKLdLj%2BHb%2FhMMEssCQNlxSpFr9GyoIuoC2g1nfX1XNVKwCRl8ochG7ca3QEvcrxLyFNs%2FIUYA9pDWOAKvJowKQ4e7HfJirVbxhJad08CNBizCqIAFji0q51QTHGcXZDyMbTVsFaox52T7Owx2vdIhfKBZ4Y9LV0h%2BraWpjC8hv3tFByFXGpffI98P0T7ldne%2BEbBSEF8QsuHawSiMTZp5Y7Iw1dqkxwY6pgFopKtqdYeYe0ybe07qxQede%2BnTsr8onVIDvIT3hCeQIGPumrGys%2FNdgBfWLBq68u24yhpUPmPZE4EBM8DryvURuDI%2BHqx2W4PmeEpriMKV%2BddiiWIWZhMogpjPO7wSqKr4Om7mRncMXjYIaMfZrkK%2B6dyXMtYXDZWxiWDwYQewW1mGuSlN3uGSGT48x65zJnb0okYPSavuRLn3voU%2Bri7SLYtkUzrj&X-Amz-Signature=91964c3562bb746279ad284725d9e8c25e50dc1933f155bba87de734a0e24fcb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

