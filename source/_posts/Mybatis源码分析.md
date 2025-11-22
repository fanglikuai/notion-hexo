---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3LETS2R%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCaKtrRKIO8u94OLDhMq0e5ghm4hPP%2Fkwt9Vn75N%2B2qBQIgadaTaoU8%2Fhwl%2BHRmSZRUU8JkniR%2BHctMz%2BY2SJPxJAYq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBz2pIt0ZeL4DCsWsCrcA%2BqGjcdh4mKeJecaXHhkGmxYI55fA2oA1qk63TJH7rpVXD4rZu%2FZk1tQuGK%2Bcg5sLy%2BeGiQ1dCrNuvQb6Gc8Z7kPqBtsmoJ0xNIiLzvH0Wt4xQZPQywG04vX2yXA0v6zqSlMYfczHpSute4zf7GeFwQzHf%2B3BUXnDV1udpUhLKtG6qsPs3t689a8c9P1xvVFTHd4TLaVI07rX2Ci23Uw%2Fy8g0Bcvem%2BLcDjV0nDAIWZdei1VkkLaoFGKbPLge%2BuC0LL%2FHUbnHFDgxdqQ6VAgpKlr020w8plfFZ0DUz%2F6dvZgwnSIvkZnPyZsp8ttBUAOTgNtQfvnrGZ4BrjEGoe0bvSnlzCvx9rRjg8UqpHImTzObvTgvLWQ8aaeLtKV6s1gi%2FkoefDxsFPdlZ1SHKinJA8hm5yuLwvO8TdDmRuV6qLFCUX1LeZsR0Rxjbmh9ebNX1Dj461Wf3k1N%2F01Ef8A6c8FP7QVzUXdAhXkEr22FDUDBJpb3GksFDtSXdERu7CHBr4i6uvGGq8EZgKskXVeEyN6Jje2azTynEfIM4wBzT%2FLZDTKFev8aSRn%2BUQYGvQZqlNlhmKkbsTRM7WXwfWWZZteCPKo%2BFQUvD8GGWgZzLiQPZoOngtIydHxOvS5MO%2BghskGOqUBkbIWXeVB4SP5CjJActW8HtHKpMuKNElOvsJGoIGHdICDTMY1A6HZVwQ2jU71jm1ex9kmuBk%2BjhfK5d6Lr02LfeQxyGk18CN2rPtYGX3AiEbpFzuKsyaH5rcgVb%2FXqqpJawiXBJZeKO8hCEHRh7Lpz1w07JP68g7Ri2N1dmTwNM7LmJVM6ingPwe2vZoyLdh7e7YddBWWsNj3hjy%2FVnNxhZRV%2B9O9&X-Amz-Signature=38a2b8f4e6cf6d59560f218638f2cd67085565fb594fb5231669ae36e4996aa4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

