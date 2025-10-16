---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CBMTVW3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDv329Ddn6vaJQ%2Fxq4yti9v8WWlQ7L5t1H4HkwvMfl%2F4AiAcSmdPwGNdixSkWERaAtJDr3RfGEzVN5pJKYTHT20PhSqIBAiO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDZtsPwIwCt6nOOJSKtwDxNZJU0P3VUsjS8Oz0HBM5MyAFaFu87qoEFhm35WWXGWSiwlAnRV6fn5v8K225r5d1qObcmCfpt%2B3xMLv7Fu4pOvBlCh0qc7FFpW4f3JAXRi%2F81qnrjUQVRe8xRfBYA8NkqvU0FaIJgUK7CrG%2FF8aM%2BYURBOGxwcv6hJQLdSEVavU2cviJhSEVaDAimYLlLE1WoW3zPQ3ULJMyP9GTsXoJotxOVwDBjlxqYq69wo8Zz3VPa%2Fm5sCil9XWffSIPXNujjcb3syDjoU4SGV9cljZF9hYaYDTfUSASiDMnOnWTA2gx6iCZLQPH%2Fa0ND9QJwPfZjrJcN99w%2FMX9hHtuR2pw7jjnAG5lDOLnLfFFfhJ1IuM8GjMrOp0x%2F5Vsu5kOTpuT6F5HtKoHRKJx4vKLaxcqmswzxtmlLyufWW70wwbZlRrOy38wI0Y2Ae27octnZ8AOKhYPQPRvG9o%2FytiwPZoqlN5bqiAHhKIN%2BD10f1JSyqp0bRFaMQCjTrY%2BMbUUYRiIXS4ZZzmX94SAIq8bClfVshWvkk%2BwrRaXBM0csIYIOs7x7gBb6AUGs9vmni1WnvPKQwXABPicAxF0M5kl8ZmqfnrV2flxcHSFtvqLywp7FeWiIxvn8%2BurZcJCxEwpM%2FDxwY6pgEK9ym4pTETf2GQ3OqbnWeOE74Vky367SY3jZvm2zYDeWCm9DrFldlryNaVaMP3kZNWJ4mnt6%2B2L2HFRFlUtnaI6X1X8abawq1OUZhmeYhds95AW3mUU2LCFn6hYGelmV2JZl34vHRfFZ5CyHe3OmWgr4rx8O9qTUuRV0IfwcnXdkjCQHb6YVqEm4sz55Fa5EaCM1W7kEN3iC6XMiYrd%2B6Z1LH9M8fL&X-Amz-Signature=034f2667262f97b1ee393bf19467edc3f9bc0993ed7af5199f9e504f3e3354b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

