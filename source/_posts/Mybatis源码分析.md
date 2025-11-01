---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZW3FTOX%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCICcdogTzK1PD%2FUqpSEbe0gEs6UEUfyqU6opetner9j4VAiAEJqnmwSHNgF10nTyPtH9ctI5BorXIJUNe9xnKWyFckir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMWdkuM1bU74JzxKdZKtwDcRHyLx9tfGI33jsf5m0V2hoP5B2thheQ0YrtAAEzOg2D3JmXH3g5gGPKzH0ufocT%2FjRNU8vX1OHNrpORrkNi%2FuvOxfjUQ8iMH3Tjc6EwPukciXKM%2F%2FEyXeXNmXqDl7hrv5464bPPEvC713dgUOtaTBqQDqg%2BFc%2F4Vd21d6QnwqMxmw2dZdNmWULa7oULuPsDuEW2bdgf2cuIOqt6vZMc42YoJQNEfCd1ICK3K%2F99MKlS1RGmPAWfXDXQ%2BBLaKp%2BFr2Nrq9gP%2B5eHKbTxg6vfjj4YyVRKzIzOhnGeMex6uaUln8%2F2faoCgmXtjfArbygtir8mbf%2F6ivwuIelLQzdzJw18alPZFTRMIPjLhDeRwFacIdsy%2BtJV5Jeby2oJPZ9xJuWPYdoQPU1WAIoTt5W6N8Wt2G48xTWpvXajplV629P5aNKj6U9ayCe8sgqQGaiKMTVVO5m%2BgG70gAdIyt59wPEbSM8At2%2Be8LxQsfBKnQgucXapL54bSNNNdDP9vVENrGM1pumx2VJZFfC8aMO27EktivDFSIk7Cmjkg3H7QoEC9Ii2hv5bzHWVkXSj701%2BJYTtNhj4BVHLD2KIfI4NrQqcQDH7xf2fr9gWnLNgdRpt%2BS9hSO9ieru%2Faocw88KZyAY6pgHBOqiGF%2ByojmEykAy3ceGIYvhh0jMenSCE7lz3kLsXcuzfpRtbJtN9%2FMzIW5mrL%2FE69Bk%2Fih%2FZIdZdSLdtVpZ%2Bg%2FTzGg3TzFCnUbFnFw1M0WmwCP5E%2Bu8v05HX0mRy4e6bdSRcXCbxFFzjz9YLlfHB%2FfpAa1lAl3XGDyqYduGoKNFev%2BGNR6vw6hERObS0GFxZXSgd56CE1bLPsSI7xGLAp2rsUvH9&X-Amz-Signature=2fb60e229a28694211110f4dde120ce9a47ecbba68dc1d9c1387c003841c68b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

