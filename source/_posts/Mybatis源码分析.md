---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WAJCFXQT%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBVCh4URjPNB3cxVBDzFpZNK4rfzmSS8FQ770jDvyzD8AiEAs3%2Bq9hM92L4sav7sU%2BCk6loHXTXI2NPzN8EMJr35SKcqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2F6iswO0Cs6FxUaMircAxTRcoBa4PucF1ljaQTOI4IgnTxtA3JXT7Zdp1ih4X6nBqCo82H81gqipip%2Fo2ZYLtxUXh2Hq7Oj0YgVCP%2BFSlXgTLFv96mHB5R3KvtMFRaNUAC7Zic7b40y6Rm%2B4CHd8VcasdXOx0dHmGiO7bOu5tlxp363Kv5w77Qhze7IeUTpRDPIq0TOOGc6enmuc2YdaF%2BEqmQNnDRvUbkss8yd4dCloSWkFfftjiu4P9Bwwbhw8x0rxoyVntjUMATnOMQkzbA3KUC7JlnXl%2FuclUc0zWa407fOekEhrOmX0DdaY%2BQJHp9Jc8Itwb9AmsdkOHbMuzZRnCoqKTXSoDarR7oZvq9pTOHG7t0JjDA2S%2BpwVcijkNNg5w4tUshGwOzOJHuzjlUlWplV%2Bp0Ws4DQezNfAe8VK6FpS4l2A0Z%2BPrqKOs7DkbWoYrF2iUi2pQNOFAEB2G9n9s%2FuJG8vzCWgGVIIklDDLYepUrg6cHWjSL%2Bx0ZHM2CnT0jnPGia%2FFleBFc%2BmUG5dtU3oltOncmB5MiDhl0DJFgOSHO1vshP2G7IUEZWNCUzXa8LQMZQOemmL91AiGmGDWYa0gapilb26SDhsxpiRj5RJi85L8CduKWslfphYZg4g%2BZP71iIbkPX%2FMJ6ouMgGOqUBdkO8LDFtBacqHGq5fgQf8NyKg36CutHyRkTc30FsvLdrOyZk1yM7UAJc1iS6qyVSfU6SVZlzN6D3h6RAwaQMTBXe%2Fx4br9umiSApKt92HX34OgoGLZXJN7A25QPl4o1eNQ5U8hkDnLUiZv5YQ6tmYMlYFA4nerOAoOd8A7Adwm73PU2gWejTzpctU8ew0qG3PqAGDaGDkjHlE7LejIcIbo%2FLqcaF&X-Amz-Signature=e37bd309b9150b2d8b43b13fc36348c1e1969c1a83f560a24e05963330c48a82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

