---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY4AGNCH%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T150057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDM0cKgagPlwgdD41DUAhn7Xv%2FyCWKRinlNR1dmW3yyqgIhAM5EdB%2BiOiv8U5I7cyDIkQqKzwG07DA%2BBUWRjVujis8cKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBVKpvQmScEc5GLdoq3AMukrcLy%2Ff%2BZNHJt9EX9Wgml3T7y6jKNFJUmM9LZgBZ4mvsO6bYKOlsSuy9r%2Bg8Y4ZIdaO6oQjFWsLEbcQzVijqmLgApZXoMokyG%2F2L%2B%2BaeKKLPyQvQGgXplyB2Lembe%2BgkQu4WABmWCsGiPG9%2BGZ%2BaOQ%2BhZb03RLYRRDB79vxlnHTIhgsY06GN8D648%2FEUl1t23ownLaQTHO1iHHhecy436dCHTRwc3zFX6ieIy7WL4czMJKYjALBZ45mNVW9jMshL6ksjULi0aUWIFdxfTxlyGr%2BLDkKGNcsvafu6SmZnfVQwZxR8p8xB43xmaLBlMw9HpHbD0Wm3%2FjAmop1EaTPuZe329O%2BA52BjCTctwferj647%2FJ%2FfzfPoMaxE4RggN2PWRfpUkPXH1PHM3OLYVCc494eUkZni5F%2F7OiuR5As1kqKkfJPvciFXu7LgyVfB72PZx96Hd9LSI02rCKW3K%2FvG4uUHve4nNz6wFX9tOHz3dwHXiaYkfxz38E2ua5bSEmy7XfSqXseUuY4yC1p2K9o0rHg7p6iEcUXZ0IoecDGqWtY1c7ffrjfyV2cwClA1jx1rN8aadzHJL9pt8uEg1wmVBT4nIcZLrxZ6oW8kMAmYjar8bkNeyM3%2FkqL02zDQis7HBjqkAW3fwES0DmHqmqk90fQu5SkcXfHIAF%2B4dg9Tno5WDIlnq7PURullB1V%2FOp7UFuupejd4xdNFAjRqkAU66vzjnhDKCglRZpNShm2SqAFPoKV23bWbPzn4Yh5Ped0dXFVfZi71sLWDV7YR9GFseemyJ2pN1w9VcAsTniTX1FR%2BZ5QP5SucwUOpeYkyDUy3hhpf6Pz1mEussvmQ7EaFs%2BY7iVLxlqs6&X-Amz-Signature=0514e370ba6ce770ae5c074a559e8a20a872c2c5a24dcbdbc52c305119e26383&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

