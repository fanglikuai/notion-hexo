---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCLQWTCJ%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhvY3N2%2FIOymW2S3n9YfGslnRWjq2evBmu7oVBkcFRGAiBTt5iQuT7FLYTnJ2hq2dzXhJjWsuiwSt1dM72i0BjhtiqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMftbvw96Efqz%2B03A%2BKtwDC6VrU%2F%2BDseMEYlhkulp6BQHt9ScOF7fU4Qogpb7P%2Bu8DbdmxL%2FxtNZXvPiiZ%2FvBfXdAEfTIBgajAxF7DN9Awf%2FVZ1wTdaWuwhy%2FMuxjwD79qGDiWHzATcGZ7AxUy%2FR86%2BvJjek%2Bta4mtDsmD5w5CX4msRnBDNUW0N1d9al4mFhyJVs7a1PTp82E2yPJtkRzE1HnkDcWDovNNihQFqr4bH9cgmOyE401DiVoYG1%2BBOu8c3rMzk56qNlbOyXJYz0L0hXW9S2n0NSjUOvlK0KSim7H3%2BH0KNG1nUW5E273JJ2txUTjpStmRFRpwOhw1CGUgI5hkcOfMmkZwUh4PTjWtVrwQnkcerbAMiZXCt8OJUb%2FcTgWhz1hMyWPvJZA0Lda97VlrYgFkOm4jYL2baW44mZvQiX6PzF%2FWVno8EFZY53ErTBWSFNKO%2BWLemle6%2FT6bWKenQ%2FwT%2FJExUqBT%2B7Qrc1lan0PG19D3DmgG88Gvje0tWZ9auQfyPnfpuyYQVmFtyNZsC6CZq05sOO2ZkMmPd8xXhpYAgh4dve2aymVhlAtiyoQwyrAijqUk%2BYF0US7UiB0h%2F4t902rUQuPASbYaTem2Rv7WgvnAT6zmNnXYx0RUnIlNhR1BPaDoxtgwnpjFxwY6pgEUeKiSRRND3%2Bitvgl%2B26PyrhyA42d0JlgHVkm5WOSuzwtc1anHqIq08FkETAwnBQUE%2BtqtYlKk27SJ9%2BRN3NlwApPRnHoPgeNj4Oy4A9dsys3rDAGA1v1ooTco8sOxfV0n6okTbwvLEHoKolilpCapfk2rkepNUqOP9YpVdrZlBVSQ7b1Kut2Vc8DQHVmlovqRw%2BWDH3Z1PQhauymZo6C%2FM%2B0sQ1qj&X-Amz-Signature=c60c0654c40c21cbd75d0d11d969bb7e79098d129835adba27a3ea5deafff0bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

