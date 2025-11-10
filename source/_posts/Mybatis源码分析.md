---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYXP6ONX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T200049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDGULARR5VkvPzgLwD17RCILATHJJIZn6SYdfBIxykypQIhAL3oswDUetNHK7N%2F6DfTUvqSvHDQt2DeO6Tjn3j10GlZKv8DCAwQABoMNjM3NDIzMTgzODA1IgzzA%2BFpOi%2BN6QcZj2Qq3AN7x5PsRcd0C4wN9e9JRrqgXVODcZjsS%2FVqvWNb9dxKdodRc1740ptBMkxhayE5f2TyBe7PbQZ%2Fn59KoX6U80uPAu9WMygd3fc1ByK36rH%2FWCqHkaqcdNt73VVwEno2Il%2FiArj7u7xqD8w4VdI9ZDEmxmQ9CR4EOuDkmQeRDfrx3yoUV4gwlU9y1lud%2B1SVmEJtQeni6HEpo0M2UsO6BLWEbKEGUsqo2x5m1tKzXZDL5bAdDH01gjvbf0NeHo1%2BB6IMnyPKHjsgAgpbHnPQS4IKc49J0E6yv4LhGcbYRSH04HhyY83IRN2ebFwHGcEZF8HjLch188KRhJnRr2lg5v%2BP%2B1jAVOoLE37qod6vFKngu7OS4NBn%2BecqbwZnSuu%2B5BC6kHiHSWlfNj7g8EO2moXEY0OMUB3SFkNhxF1T7iVVuYgL1XSFgzHaX1luySuYNVDZozowIyS2gf5boG6uQ4qoD5tfS2bSATQnQ42sBRHZZxqUwB3quZrQj4lJqFTznByEf7Kw9Xmannhyfr4UkYZNGtThANlFTXU%2FbHRsMc2aPB4kaZ%2FErAfGkaGHYvAzTbqDVRYtdcGQmOJFnlFcSvoy28L2utxD3a%2BW61AGUq4TFlfKW1aBA%2FUJ4SfY8jCd7sjIBjqkAVbT%2Fr8fcnzRh17JCU5GK7aMoqh%2BM0M1EvDoJpARTqKTPYh6qvwRF%2FubSL2Nb4ZCxshpQYBgm7CcvljusJaUlFTOfVDTpLbwWXOhviLHEqtnslpwivEG3PQuBojBdDNVip8PyIU7XpC83sHHT5dcRUnfi5W8ZY1jLM0qVFcL8CSJedzFn0AnB0GD50ReJmjydWDogBB05S0XOex1bVUNscWcqd%2Ff&X-Amz-Signature=6fbdc3ea634c1b231e0a9605274048af160f5169495df1292d29d02e51e3b5e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

