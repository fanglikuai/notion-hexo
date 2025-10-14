---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RNTAT7C%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T090104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCf547U3zbnlXvF1GZhA69wdhzU7qG%2FEtirVcKRPVOINgIhAMrk9yomXXyiblDQUiNfTcVVWtG147vIEfe4nxfHrdS3Kv8DCFkQABoMNjM3NDIzMTgzODA1Igzh6Mq8PYA9UBAXVRMq3APfq6TMkNXR%2BxJDGnBPlrrrW0DijYNf79me7EJZhpt15r5V3OePo4dhlp7KJUMr0Tuzovjia2vRT9P4RGMfjV8czLAXrsCWoLUMqT6xiPMsvj3%2BGSLblqUaAiRO0oIb5lOpW0UO1v8IyGgzou73rZ2HOAFOjuS9SmACCV1xpwiTk212zWRLtxL2cwEsr7eulp6qMwFuFF0GdphFkBjzp9y%2FMvgNL%2Fj8UlptJ5wSMJW1D7z0nSh3WnAbP3VU4Kayj%2FyaVUv6ak1seFpBko050jD8gjmRx4wssLbx89UQvqbF6fvr52Xk9KWCHlidVuN%2BpTsJNGG8%2FiY9CCZ6PHf1Dsd%2Bb7ZxjBnp9MNQCW7Nst%2BBiR0QKDHkvAyZBmoE28epBgRcvUI7EmymcTPLsf01rUcN2NPyZstbFscIVtWZ9h46ZtKn5VdmGWebk59h2XQJzl8LCW6mXvq94guH9jgTAUf22rADgU9bPNC8QEfuAzR%2BbRcyzQVHAa8lJghCt9mPBrsxnh76AGWIiHMM62UDfrlYtvI6nFHoS%2F3vhTiuBZ4jyOkc89WdaBLiS2k41Zwnvv5JItgtUBpJ58R%2BZsa%2FqjZ66C3nqLNcxAZ%2BBV1eYlL6wxueZlS2s09f7wdz8zDfgLjHBjqkAVV6Hzn3wqtuEHQiWABOoRNG9mAYUObOZc%2BVp6QGVcAcpLIJ%2FWZVw0LqXvOG3zV3bPZWRQOLfNH%2FZbsbeYakfE%2B%2BlsFZ0I5SrQ7FNRJwuNn40t%2BhIr1fWc%2FzP0PrXSXwzVKclI%2FwWZ%2FHVba2xKh2oEKuQqJd9Gw54mf0pzTtbn5fQiEw4WH4OWMoCP0SdnrDNdXjCwoG8MW9RilvW8knj1RgvLkc&X-Amz-Signature=0f7c2132cc99d89845829477f7e7c97dda77eeb08b8bf91d796ee2049c2575b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

