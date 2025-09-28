---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDJ3MEJ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGy7LYCTwggoD0KY7cF0wDd7%2BVgwzCqeVPCL9e8vynEdAiBRsD%2B7WS8P1xzWroY66i0Rqu79qIloH8UCKz8q%2F1I1iCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFhlv6EtaK3%2FDoCKOKtwD%2BxNgO2%2BZxwUm8ai5uvj14jSVT8S8ugelVCbveJj1DjMiX9TNiPzHkBuzai6ILHUxb7Um8JmAJgf6FHCDs2Khd9RW3J%2FFLpP6nndwQHe9CAkeM5OAi7McudXDwPVMQs%2Fz%2B46KSpeHAVU%2BPceX36TRm4Qt9f7mqjH55KeJql99slF2ogrplqEH4XMdJFVQ0u7WXg5y%2FKQMCDAhvpG9GWe2v5cXMgJyqD7SmS6eoje5gRIZNnxmKKTOC%2FkUkOWHx7HXp%2FM6eQebAKFYdP4Ilqg2AyFPGEnaCELUGEHSCvMnQCPcNTj%2Flx3ub1r1GOIArUe6HT%2BjWZ%2Bfw3uvISl5gIE6aJcFmweycPrbNcS10e83%2BVkULkGXzKFUUDTgkQEjIE%2B6RqX4PvgiGAqOJV1WzNkm9OL88iGiwdbvWVU3uAI7bIK6Hc0XbSIkxnCdiTKMljHhXxEEJo5jeWlrIGo5EeTy9exuwld7rEiSoC6UyhafrZY8p5mpK5gR9Vba2DqO4UYPWZyptgkFBgpeq2gYXRYpv7x9nabpyGAJUk42dF8TPNmLtHyWKhlbId%2FL5Imt4uK0EpYPC7geSmbAHzijTzS6e0XNiYZ3eHwV1WnH0M%2Bo0P8G4hRbiGUuDz02X98wwr%2FkxgY6pgFgvsO7B3sbN%2Fn09SgcYSQBZV96H4GMVqTDmGMtmeb6eJQ4MAlYluTf4nBgI0xSDsBPGM8Kdtk%2BwxKaMPfHP0biHTBAEnQKlqnnH28Irys1kHxRDc9HJAYvn9ZrWQnXnK8C4NwenBE0BQ%2FOexj0PdI6tLB1kwzupQZgivGtsmKVIUE5gLKAE8A%2F3WlQO02uS9oZADHZhophp5DABKQdWTHUySwbyONF&X-Amz-Signature=833fdd6a533c52c9e667dc6a474142dd193d9e61799fe6f11554a6324b5ec7ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

