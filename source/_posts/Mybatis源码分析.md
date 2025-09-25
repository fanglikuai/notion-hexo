---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEC75CB3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGoAaE10S6AryU0VW3O2qGE34sedxXClT%2BkkCOGeiJftAiEA6ff5%2BGL0IWL%2BGmkIqpMZLS005qKpBb4XeDZf6BkrgHIq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGDbOxXZQmb1Fy6RRyrcAxrrq%2FCGFdwHx7%2BY9fQ%2F4h%2BdLygxRhOk1gD%2FpWUT6ooZfcVUWSLkC2btQoesOOGV316LyC6cNZRpxouyXrx2X2xtvj1uyUL%2BlC9LW2OP%2FqbOfuO9zpibqz5cmf7wZjyQSC7vO3l%2BHToRVrqn%2BJTqa1jBIEy9ClAnvEfwItHhbwZ4LaiPYk89HbuNh93ZirHez5Kso0kG%2BH7PW8m7Gdy9QfV5UjK5wCaJdZWCE%2FHvSlDrGFIzFBqrJy111JxI0f0vdt0yLobmdAarBWsIwrVarhT4qszApLEMPTa84RKpzeUWGo4VJqJZ3ymIDebPVn%2FlWJLuPnB0KT%2B2enlAc9fdOA3siRrRNZCnqxecAM9K7%2B6X8FkDfYmEqyVpM5rJWtY1O9n8pqkYMpxPOEAEHqCDAysLyjUKvquJ8rcyaxN6WhldxejxxP1V2gMdnZrjJSYqdBK5De8jnRAsI24ZaOw8oJNLZZpC%2BZIqs4NPinf80%2BvS4Wb%2F2mXD2%2BUl4RS7uAKrV7s3e1CHorfbCche%2BGjTcfY2mOGQsz%2FdHxHehTEZdOWovxQ0vfmpo%2F34Xg6OzvVMDpyomg6u%2F6Xi5wXcqgQpvcIqoHJBQXJijizvwHMvqoJd9l4Cv2GNAIhWTFogMM311sYGOqUB9mIAYbvl3gqZR3OUfjEkX%2FQ9hgW9S%2BLYFWBxxbZlicVmmRlQwVkJ7ViJPro7%2FnRogSCncap5tcNq3J07Rg8HBcLnhNED%2FYf4TMGL%2BVFJAtT%2BOZAux2AV8L4tmdsv%2BzWiLt2djoVoctf6nSu2NHgjaHD3wxqoW0%2BFUq8J9JFaCNmltx5P8k0gizdUQjwM0Lty2VPyy0GbSWPpbtEXdM19NVjUe28y&X-Amz-Signature=841f3199ab9cc0907e7385aedfbc70ac7db3cb5ea1bf5f0b03cd0d3c02f5c5b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

