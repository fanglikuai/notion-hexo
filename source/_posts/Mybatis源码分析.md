---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PTRUCWX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T070109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCIM6D8dhI1upCI6%2FZu%2BNikzUbfUUjMalwOESJBhJdt2QIhAKkLoxbYBMzwssgzuLVFGrYapZ6%2FmYtJFrqQYkPH%2BVCUKogECM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRKDcsVS46bozEd%2F0q3AN9DJtQgUobf%2BiM1%2BY1o1P05Hrj5DdghkIHCYGtOknAOciKdC0KneY5S9%2FH1u9LLo8YZJ74JH3xPIQ99g9Jgra%2FFrVVYYht76ncMj7w%2BCS6nSDcbyHjHnFzIjbkQ0%2FdFXPWwxpktqiC6lvUNbRKNjUVnsaYbzqBo3meHDz6J8e9AfALuoFm%2FWp%2Bu32j7yLXsQICz7H3EBMtw7PR2M%2FbxKbURAOcWp4pDVUGUEyv9b3vhsMaJnc60zR6dAs0biwnbMXIBP3VW33a3m16UCWmE4nsQugh0580X94Ay3kKX3d%2FPqi0RBt23WnvlDe%2F76fTgox%2FoZp0QHXfaR4dZYBoaf4N%2F6QYaOohSwJP6p%2By5u%2BTN%2Bpe7CoAqFNTfjvqdvQOQzGDcu8qZeblfUwFC4tt2RR4uEwjAZ4Sb%2B%2F5%2BNnWcJDFtOIb1vmIj5gUj%2BAgqY%2FZ4dECBY%2FzJehk5vEpObcA5drP8X%2B34xHgUjNtwmxP6JkR%2BmIKZ5DoX1XWKUPe6bgJWEZw3fnNRX7GCEFHPCAt9MZAJPcV%2FEgJ3KOCj9Ni7VQmaNfQKwKk0TlARyjomqvGEBUYHc%2FgythWsx66s6kn4YDrPPTRxALOpvQz1wgIRvqvDjzXyypYKlt9eD2EvjC2nJ3HBjqkASiorV73D%2B%2BTfk45lPUToTZwPYMvgWVdihXxfRb5FaY7m0FkHdjPOhALQqC5%2FvOJvGMppnGsgiV42m0wVHRB%2B%2FOvNkVXUSO96xL53ilkpkc9Q0mYLDt8ABbBsKKUx6R8jcIgXMONJNR9KZMRG3mmPeqMNzPCzE1iuo7qIbWZr%2B0ieThvlLw4DRUpDPo5jQ84VxG59%2FOxXfI0XyTjJBvBmRtSThnb&X-Amz-Signature=0a025351005323b6cb510d5e7040cc4ff1e3e1e4aa67c62f22468bfebec72e77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

