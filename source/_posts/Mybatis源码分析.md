---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664E3IEFPJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDhx5OgS7ESESxOWaQvIjQDZSzA8htN8x6o%2F0VkHANrKgIhAMRg%2BU0rRPxzKd7qhL5d6k6nHk4Z5%2FAPg7UtF3olNtBYKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwyBBMmHTW7Tg5AVjkq3AOuYwd6CWsTRATdxZKTBxztZT9Osnk1fPsAidHjQW6e0itUrVf94P7mgSbnxeGfA5nr4D%2Fy175JqmUvEQ%2F4QwEogPEJnKIm4HpUt7BQPh5weRqGcJvj2o2TQuPvvz03cCsYH5dH8gL%2FujOjZCyOC6ns3ELl7cEpUMerIuQeHRyi8LT6gXL2sAc7zcHwI92UzIaOEHbwaNoAxHgud8x5QWn1anzgVJz7czL6T9jmZltFFjBOoJ3MFR%2F%2FwkZHMTSLR4hWrmULLJnQpqMHiyVsBRcrdzpyqJj%2Ba3xqOtEeciEyE6%2FBTsFszGr53zMPKhoWWcR84gYf4HRe7SBWZcCn3BMFRwgiKV8dGrm2cww3dNEi9LgTfCU2XQRGX%2FtPu7xoWF7tJTQ%2FOn23ZguzGDJrXgopxmfg5gatLIfKNPgaXVAJxKW1MD3D5RK9pfOgfdV6qbELN8zgrAOzayhIJ7nNbHdQ%2FZZ345AytFDgMZgvR2bvoCFcsiOZUCk66a1pz9RrF%2FQX8uKsitM3OWR4grCtJ47ThqaWsDzhj%2Bl%2Bg1X7VELdFR1yABloq2WNM133UBlqWIx%2BiuwQjyxXkPy3ammXS9sZIiTZWmvigwI%2FKewT8oGgGBAWX%2BbUH8JKbddH6TDi5MzHBjqkAYqedhMlYKOPTmJ1FD6dWjcwU0lPI8ASE%2BonpBr1PL1lu%2BBSTqmXveCtc250HypJF51WuP3Cqm2C8PA%2BCkDn%2FbOQN%2F4I%2F2I3VAyiXc%2B46YRMrnKFpgM3zlLEVcJFskqMwcmmyV2Vrabfgn3qANTOoK7b%2BQf3NhXUiVg8iMV9u%2BHxyQyfDWxFfl1eTGwdDr6zN8%2By1uvQhgfvjc%2FwilHeQH3mW3cv&X-Amz-Signature=e5f4543f4561fb9fbcc0ca6091cb7cbd8a9b33e06c3d2ed8380a3e9021d7ccb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

