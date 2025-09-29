---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDT4XG3F%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQDb6dgQbTSqGRVYtdUmU6m79uakrA689XuBAz%2B%2F1%2FwJjQIhAMkCeLHkI12uFfZ8eA0LPSG8sr5LRrrb2F00J3Ssx6WCKogECN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxlofEgfblPloQcLhwq3ANGFP9DiSBk8YJvB9%2F29jjtKQFYJ7AixqhUoJP2O%2F715No5pgXRHTOhrnrRLsaraCSFPI47paaC3KszTn6JSagO3ivRHpUB8pTL4QVJtprFCjuu6zDCbDoS0e1KnF6TYIWbD6hS8Gqg9odkr7t8Uc0AxXkOmI4pp8%2FSftMIHfTpJkkt5NXbwvSQhGii9tMwlyv6lE7ttTqRfeOaPmdIWCAiyWjAbJqZrTYUHDA6doSmEk16GqXeQOWKtrCP45qqx0A5c2ArMKPnOZURWEHgIc%2F%2FcNK15aBoZwIeuv4srYJC35qJuXc0e2jSs402F2572cUuPg5B8qmvyE8aJKLAlZbXMPhEoUhIx6aG6CiwxJzsDE2hviAh4rt2tz1vt1zim%2FdUSmQbf%2Bx3fHQTbmsXpK8qcZrm4FndsYU4DPK7E%2BVZ0ak%2FarwbEaHmSgmipHkVTLAJF0NJYWkWmsUezmWr5mn8EryMJ35dBA04y0Kx4GCKd1GDBFO2f%2FSdDaff4WTAGXKpisVKaqCSBRzKZIlB17AI5gzYkjBWSDtdJo2Wcj3B9EVtp0LUG9VEYwO5US%2BPW4h4NfwM%2BHUufwtI9QWUbqRE%2BiWrg%2Buw572QzeeS73La7aHHY17R6znDLPk5TjDkiezGBjqkAY3AeXvf2YlsJOzFUeyqBL%2BtjbT9fcETNmrK9A8m%2Fv0VE68SYAAMauW9pGWwtHLFZUrA4225HmuoacA%2F5d8%2FdGFa%2FzXzUKPD%2FcZOq9HAsfZcDCZwDZgLETzfz1hVf98oczNc8YKmWvlBmvfXQOlDbqV2oJSGHAvsH%2BP0vsgpAYJ9kT7B9S9h3otGcuTCXkugQHO1UMFoi3ro%2BoiKZvaOxOf4uAPh&X-Amz-Signature=3f9a22fd48c738911b814547b13122bb34e04ee4a198c42d10883406bca8d9ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

