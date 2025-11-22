---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YPGFG2G%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCdWHjDKjXifgtt8AekZeyaQWDjKvW2TPN2LcqH12rDQQIhAI2wSTjWOgpvmFuJvI2OCmGXpeHxHMwNJgeA1elmjttgKv8DCCQQABoMNjM3NDIzMTgzODA1IgxUln808d%2B6eQXOrvUq3APbYOmgbDRTo88fV2u8qcG9JbPTmv4VTtdkaHoiP6t8b4genx%2BnZdjkzJj3D51vctmJB7%2BfVKSqaRrQMK5qSIUacGlLhb4%2BoQFV2Z9EuGDA1YlVeQZBAebggDJHNbAbIpby2kmCtqWCfM6KS7tlPgTJGCliLz%2BEcLgbiRTARQjpIO2wpNqz%2B7XaZESTCpUCN%2B18zWropFaTDa5MRfhjlovj6MFbVZDpIlOsFfgZtUkdi6OMr%2FPUZz1h9Ctdvrhqlcm6RwmBYe5U7%2FTFoaP1lBfFPaBAKHbFCjCtVn5KlutOxu0X1xvCXIt%2B8fbn9%2BshyF0TRJuQ1J6%2Fhlybvl64XUUWqeWGIlVfLvUmc7T9p9DJ9hD5JpUBEU7ca%2FMaXJSmYueQ%2BgehGY3YQcJ851xGHRYsohMEtgAgLPxI8Y97lfM6vmSoi%2BwDTRHOTTUwWhvVmj%2BrfHOdUAKtM0lUiuQqs%2BxKkgiUSE7tIPYj6QRMOrjvDEhBpwUaEnJJhSosDGcb%2BMFU17R8FDV2kKFPXIOtZSXxDxzN8w%2Bb8VbTk53%2Fjl57dBG5oMR6bQN9037xttVjD%2BiZtrK9VwriiQ1mRnlfKS%2Bmn2rWXjF8JgjxcCiDNxif6rd0oRFRfdRB62cAwzC2o4bJBjqkAYreBpDWl7OYdkldiRSBbvv2X5QJaP7l7m7lYfIyDzbWB6UrXxi8sl0Z3c1TqN4%2Bhij1a%2FPuipFlyMktNUt7RYFsxNRpLaUml7TkdDejsdV6tNn1keKK2X6%2FaSRG5ccormXBIbhk2Q45YCj%2BxVVQHzWmmvaDuLYGpeabLa7DaiMlRXxUX8HeZE%2FqawBFc%2BmxDd%2F4fdHJiVQkU8XTbANqk0i9g8lU&X-Amz-Signature=72ce267afebf0ad38298bf7526fb74c1f240ac2bd87aeeb2e5949a88e7270896&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

