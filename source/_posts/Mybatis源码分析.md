---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SC7FXUZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAW5NnIKBXo5Sqg33pEbBX60ZCIAPX%2BsWPHCScDJngIxAiEAt7hsX7Ou7cHz4QYuVlldIwahU4%2BMpzXImEWiNyelg9Uq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDBzmmSOL6KfN0Nu0pCrcA5KE1yf6NaR8bSH6%2FDdf4WeRnaispuGs%2BxJZbWNTA8iG%2FQTIcj%2FeYU0gwbZxcQfIPVKOwjoFwRQKQbhWpSFOMKLgh9OhNZ6Rj1MbVaMZ9%2FymoFmSTnL7nc92KSWQLiGiGkVYtWufKvvoWj%2FRm5ZvpoiFTT%2FN3X7Uxq6OS2oaZjqIA9jI2LGxrGNS8Kko6nZwbnuaJAanqoQVUhn1OyplBQEupyngnu5EawS%2B%2BWR4gWAo%2FxQi%2BcnHdzsa57MnCPlH6zRi6kDUgZPNd9NaHp9xJhlD3t%2BwujymQbeVoyeWb3hSLieTHrKT9NJpy74SJvgwtUtuE3tPzmXVM4OGUv7m%2F5s%2FtspUUqiW3KUla0%2B1XFGfqeHfB5CV6sAan%2BBdecCmDOwE1N2KlarNpSn8QJzKnITN3OQOUYtxh9ClQTIAIpW28DqWC1CSRptgo%2F9slv6f9pTuQPV4iLKqdbl36ovf7VVfkIMBfvcWpVt8ugUmMmxSwGXlc5osUj4PHCWZgBo7OOKLZuY5wCH1r0Ju4XFhH%2BTmb71PBQdiiU8f7b52SZkhNfCxcRv%2FesH%2FNOuE6fasZdyCtiTgvQtv45uKLJ6MnbnUKFfkXjmRqwLFQgUUYXTyGqFj3uOXlku7d5d0MP2kuMcGOqUBCXa5h4OQffKbA2EYfgcLLEBSXqe2BlVSMLRhCd7OAGsFXoq2XbRUjFbbx9BNXdVyDlHfk5spslP0Gm3Gxk1adVbQFlXj9a%2FhByX29uAYGwBQA6gMA5KqtiHryGo%2F6ohQ9BR1kjhIeMhzEhRzZAe%2FuaTm9vIKRsGV543Rn2evKg9hJPUqmp6VVFYdZqaxIus12q1JyzXXfuhXiAqT9RRIlzOiYc6Z&X-Amz-Signature=1a617b4c4643e7c8b570abdb49f4b3468821f9ee0490ad78a98a09b2349d3f73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

