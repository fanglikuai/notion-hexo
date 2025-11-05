---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRIRIOK2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T140058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlRayQNWihGHJkelA%2Fu6tSDNFhTsakkzY7faCZOIwd6AiEAgfGqIMilknSq35Fl7XjXecHg1t1wxqY4hawZ6AYUDNkqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuauyf2y9UCNAWPeyrcAwSS6mj4RB9dkox6jmybUb5VgWmZmlrucuPFcwwkvK3HF1jxShNSQqmesHFInuQ0b0o0KW5P0w68LU708l8uIvapqMRWOiXLyA4f5F5NPTwdVqg6YvwQYoEi5NzVvqgWiWQXcSQiOIjawOvFY%2BkllcDB0qsZUxUHRF%2Fs2ccrdiOSEZu1qoYdoNTxpxQXeq9C7n1Shv5HFFJ6K%2Fj4abrZpO19sggAahXWQ21uQ5pdbBV%2F6yOyuQU7eYFSmWooYPhyyJWOXZukFqv90Wyt%2Bg5PaopJpiSKmQ14WtmJMw%2BM75SE3fr9SAdYYn7CMGo54JQh8HFUf7sr6DQDIz%2FcxWdz3TrRZzPdususqT2afQ82LwHaJZKnEFwRs2UDQZRoOiCXTUqs%2B8TMYNQy45OAeEHx4ThRlXs4t496kSdZWn3uvMNcKO18s%2FeIf52vYNmYFiENCy6g5iM05bqTANWZV%2F2h8GSvaA10gjKdofjt766teHnnezJlILvpjXDIVKbomz0dqyyX5oJaxwwPk1%2BeW5k0O0abIKGsf%2BKK1n5g6GUtikLfRvhb3BIvGYrxrlzwNtLiFaTHnWRQ3suQ%2Faxk3wGfN%2Bd8%2F9b4pnEs%2B%2BTMeOwWWkG0Lb7BiDykPLFSUtPvMOqTrcgGOqUB4ZQkytYTLEiPtE7SarXUti%2BFFj8Ti8YbN0Bsbzie201h1sFgEuJgTIGQsaZr49GMW6ZOkyOO39lGhs0bmKf59UakryghW3YcKHKBaW078QeiD3%2FBUidfFaEwWk6IV5kXlQS%2BwCGrbjFRdzXvCd9JCkx5bgPMNrQ3VchxrgRkQSRpoac1K5NnXRJVuZabSMPNreo3XwrKJezO078sGXGvZ%2BK%2FBZhG&X-Amz-Signature=96ad67b8157da1573c42aa62e69a5cfc0ad8b1f3e923660699fbe61354cfae8f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

