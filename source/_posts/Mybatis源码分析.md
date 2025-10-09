---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JDGUL3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T180159Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIB54f2Vd5oKIXXENN3SqTmnZxAeGzcMcfSTVCiuK%2FHYWAiEA7zXCaOoWnr4oYJN6eKx3qOkykz5N6JW7eAsabfj9ivwqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIGapU3aO1nSBqMF5CrcA1WAk0mVn6DkvEu%2B4RjvRq0AUR3hOJQZOnZ12xqr1ZFt2QLWRrHmktHPDZ1OqLcU%2BKTBOaMmWSN1yNLz3fTmGH2ztPiBr2LTxwoC7WtdPih3gHEcY%2FkuX3fhKn6ZK%2FC07%2FBsNWXws5fdflZW9l7WXZnf3L3Ecs7LpgVD6VLJ%2FB6oybm0vf20xaD%2FaAmsg5yU90RC5HhkhCD0DnS%2BfaPscYzC2KQ6jI0owIRzEpDc9GMQSZaxxKGe359cZq4h2yd%2Fq0WoSMdqpyO7K10B6CWtxicbw5WkAItll%2B1fs6lgFBuAdICadzt99miWtdEjBWBRmplvp1%2FwOexCF0%2F7J3OXE%2BZnzA%2FsqpEtF%2FPEXCVFm9m83EwEYTHCPJ1dH4VQ5qsUzEfuDLl%2FFlnw1CJkQbwLCkH1FC30pqkYM30lGMUHNNZOppg8kxCxsman6xAVsnbYqtSv1VH%2FP9RkLuu24U9Zf2266vLlC%2BO%2BbFoTRnWedaGLW4BFrCUUJe4x6w5hMnDEJwaS2vtqIWtrnczjEW%2FkNVhiafvJQnRcFNHy5cr4B1ygXJoZYDRe%2FLy4jnGYqDBoKfnTOjOd0KevDJvgWadWvWqNSux33K0qdhIbSoK2wE8%2Frp8fMg01qM6pEAxiMI3in8cGOqUB7h2kR8Sk4x8d4GiAKPN0hqv2uyJB0Kbb05ks2STuUjNIW1Z8BLcPoxFHgw735BVQLmmcWFL3wIibe%2Fh2ze8lEe%2FkBskFAx4LjwKum0hXtd0WMqBJoA%2BOKLOk6n95JRvULbFyJ1rrQDVOHYyxXxzEBqZVqeqrWcaYb4Bkm5wURJxujocoPA4epp%2B7RgEyPq9YhNlVnyB2ct31whZU4cIk46VCZPgQ&X-Amz-Signature=8fefbbaaeb620e34137c5b8de48316ddb31f638c9e07f2c4abaf24a5cf89fd3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

