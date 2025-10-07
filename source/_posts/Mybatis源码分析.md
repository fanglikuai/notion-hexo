---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYBGGOM6%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIBA7VbgTvAaWupna%2Bj5HPBvW5JzVftOpuXowVg7r0iY5AiAxrjW8RlLn0PfFgU%2B3Gc9kWckkZzK7DQYtEJ70COQ2MiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlYZUNLLxwWGO%2BjtUKtwDUiO4YH3Z7ArgHtqZlNRP8UN6YycMMGXJBW9VQdsUkGYVRlPAFk4tyGnn%2BzyaUjrBg9JAZhAmTWdWPiGodJ8IkpCoV4EeLqu5So%2BmZYj7NXBCCSJayAjEScTCrUZW8oTIxiaAq19aoASQqJDIljuISGoq%2B6H1%2BTb6kvaRiL5KOh%2BJpAh6b9eZDljIbcSAAfm6P%2B84lAOulrmN6%2Bl%2BjuIlfuJhGg%2BfSoKr%2BgRdrrAiXUr8BzHuIwrXz%2FXqlg1%2BJvht%2FJ5PMT1f36QDBZQePOF4jYOraQRXtD38nWI11eWXhDFmEWgRsWfssYDvM3aOwGC53LuofgZucSbZq2YuymgbGnhikJyMYqOsIt%2FaU9nGJZOq97rbXQshJtsPvVY6g%2B1Qn4FYkk9e47V3ZmZvB%2FYA%2FljaUsbeAPsvreMg%2FkoGgxsDX3NnT8FRD1a1bp30raApN0NThZpo23NdChQT%2F%2F%2F%2Fj0jG1Ugb47wbkqG%2BUsXPoDHiDOAJDdPOBzlRVXpa3eXQdbJm05SYck4cxegIyHSyNjrOc6%2FWmfBK9jTVQqc%2B2qRJSUpX2jOWCexNrXeZ9NiaWS1iEWD0%2FzcKN1zFEfooyTxDQwzEl98TSwutiDibSskdtJmSQOW0%2F9jXoD4wqYCVxwY6pgHlmbNWcfwsBYs%2B34H8pL4elXgWLri87i0faOFgoIJC5nUzesgPWuHooNz%2Bvw2EsRk%2FtMemDbySamOWEx4fAD2miTim7UgLZ05syQBmGI5%2BCU%2B6ac06kUC7sOT%2FvtMjKkx3RVfn4Y0pXPwZP%2FI0mYUaAaqwjjcGAaaakgE5Z5Z9jDAv7RH546TM6i27foyQjl8FtUhyofmHvkBxY1A5GRrsP3HT9uZW&X-Amz-Signature=6584d1246e174bf9d89dbe194b11d458f7e22730806210c9bee547b17bdbfe7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

