---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVBV7RUQ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIFjzX%2FGyjZJrUumyexRcvduYLe%2FPuaJff3hK3GwGixsaAiATs4FPtOWMCQnkwQQRnq%2BG7OdO9t4gQ%2FtzIPoldmXuASr%2FAwgIEAAaDDYzNzQyMzE4MzgwNSIMUYoTXV6rMEMECNaDKtwDwRYVoTEtS%2FsHtoGNKqji%2FHNBWLbFW0Yx0aJBc20RpLp0z%2BbujVdn39H9FO7RX4AF25O4zEy4nuuKoqE4Zsn7tHrv%2FzdEx1o3Xs9SHZ8YFzuf84MgHIKGdndjbDisR7r6j4KBJV4OLkMfbj3y7XaD0bq5B6LbALOWZ3hf%2BEXML1pQKGuPr5BlqjgyA%2BZnY3jDG6H%2Fnaqccdnumvqvv2EP7Kwzjzs6aW0aBytJLcOPm90ySjPcmzJvvg2O3VLMsB1touKzrx6%2F%2FwU%2FabDQi%2FbLDOkd8fZptfmPQ9yD%2FQb503BuQsWzPGZwYU%2FTkT3p1rooUzG8GHIwaZymd5zetwo1%2BpsjYn%2FLbUkM3bNo0Sx8wgxBBcOLtdZwA%2BW1%2BW37Qw1pP3J78oJ9RY8%2B8E98L64zhfxfvejNLqtS697zYOC3grm461HzhvBD%2Fdatdnbl9ixazSkTzgVF1E%2F7VzMoBG%2Bfn65kCjm7UzNjsumr46AZE%2FfHvl32GO7qwMm7J%2BH8MOn9WyAJS8vO9EbxRxVp7QTpSTHJqn9deXHYV057rCZdJqCL87iPmqjw67bczdpDQVhRSumvBmUI5fcJjxMsA6ICgFvkwHlwB64AkqCbmDzkLQMd061ZBIZAwmabGQcwoezHyAY6pgG0PeQ7En6wwmIabNWRIi1ycZ392kodX4q1jJ7UoHIls09cES8Nc8c6JB5XkENNTyW8xuH80gv%2FldEfMv1oHAsqY0lK5FOJgXlK0chT4lTd9NEO4D8tsFynMWxEPRXeil%2BCWUhspZ1BHg6%2BaFnv2dx3zgsmi%2FTG40YV0xRv5k8rSxZutVRTzKoXRnUD6cT4gbbol%2FquLUQd7m2yTeOmeguwDpy0d2Xu&X-Amz-Signature=02d1bf410d411460778a8e62bb8a20b577a185616ebe3112f373994065a045be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

