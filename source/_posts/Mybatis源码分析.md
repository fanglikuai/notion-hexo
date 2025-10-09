---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7R3A6QW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T170105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHjlkqMi7xefwZ6yvzuRh2oBvnpyP%2Bdj3YVbsQp7QFVAIgXW2Vd93eZdx%2B%2B5SjlJOpapndpAzc%2FGEztxIszFb6ssUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkmGqOm7aVt%2BQFASSrcA2odl6D%2FMxxfYLDwZBTvHs%2F7PZRnfeoOKhDCgT8656vygDSP8qII2%2F%2FOMIqxO8Kfcn3gvxm6T5hfyziEpY12ZRGKeScm6pQ6ttsndzOIWI5TKVGFjcnP11HUFFe%2BVlLMjRGwdCamh%2FvOS0gpe3lB87HHYq4xrFazAGFZJbwCgP8uEMhjESYsFuI%2BYU852MPs2tAQqoONxDG4Yod41X%2FkIhfzxmVeHOhQFU7n7y6KVHBf%2FM2Vzw4aFv7O6P6NUYoFZ57spEeUlo81nips42zGogfHU8gPquf3mQALeXaUkWnZwU4kVkgM7d3MfPqU9tgqPlU%2Bwao2x4ajmSDXtfcdNv9dRoh2IJSbvnKZSjkjphCspX2mjl%2BgTt07Wug15rIwTwRdThe20iDggIpt0%2Bu3eT%2FzLac1aNEnRbHBWvWHbFiHq6ioO6x3%2F87UpAd5uiFjHb%2FWcczpGIJ8uNtMTcVKyw8Keb%2Fs3nHX1BagAuA8L8sViL3ulp10BCgTDVWbidAiWnMwWr%2BziemeHeqaUrOlgaWcAUaU2ECzg9qdJqhl3dXdvivX7PuWTn%2FgXvANwA8I3b5QeY1pQQinK1GGOPflt6j%2BqLSraHF5MGCzMWkNHWuresBjQHL5%2BK%2B9rLFxMJfAn8cGOqUBJhaknO0I610B9jMv7IeUzN1lntUB2qHD39OXdvwlQBb1EdmvMikUd0CTUgfLAdvxPdcXda64jDyne%2FBArAiC0tDFAn5nx3fmSRvCDGYhWQnNcZyFyOMhBbJaNFUe67DAhJVBlQMtTwGLqaRiLeT5mrXaTo0vtg4CXfQeMGKxzxrzAxptF2npj9z%2FnhJiD3T30%2BYup4wmGk4dX%2Fc0csQoQqmYFopN&X-Amz-Signature=d39b1f04bfb01a358ce4b0d3d6385bd25b5f41f3349794ce2e806252470d807c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

