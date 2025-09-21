---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZEANCU%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKHel0Np7vx91I7eZYZiKqDaj%2BqMX3HKZ4hT9aBEL9%2FAIgAfLpGoxveRl8o%2FWoufqiGe%2FSwh3iiqhbwerlJRqme18q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDAPbfGWy7pKlldau7SrcA9Z6bPDXnin1PzcgqK2KDR%2FecT1mmfp9GxhGf14skV2twgWzjsrh8JJXaNThHd1WFdG15frGOVJOYdRzk%2FHRKa2DPiDUlK6zUjsjYBTtFVxGJBtNOkdx9TdSj%2BGc91ZG7db425QfYASdLMRqk3TWCHfnv8GUPm6BdsO76vt1EOhzEhzXLZ8edZTuNJheM0lqfnQqXnzUwmQlBRqP44fUZfF374B%2B1u4rKafZTH5LZXezYIX6IhFavrXVKyYkRqs7S%2ByBVSAzu7f2sNrZ%2BVaVBAvGLSF1B1WUJYHadz6G%2Bv2i0FPRplcUm6aT4JCR0GG9hr0Xf1IVvAqwlA7luQHiOo%2F2tSlaAcz69B2hnoa7Wmr9O2gP%2FwksNsG0rMhktAoiIk2c6At3HNP0wlEnfODqqInm%2BiViMGvTVsiKWixVGMC82TBZ19fSj3JUIypcpW2ubs2Mi9uHVAgVock1QVJd6xYBeidbmmgq8CYlOqdPbW7ca%2FuqbSVr20tg3nN6XcopdDe%2Fnt1bqpHZm25akXcHDAokpr1fKn%2BSyGjkGxGcdlGvCs97VxYpNUfquz8CaiqOswRSNAPJv6VXOOGNu3%2Fc1ZbREIOOj4eP3MUCKG%2FPG5x9v6GYp6kSEuQN3MWUMMDpwMYGOqUBqgEWp%2BGcepU%2BfsOW0WMFkIZBwJpCvKTVKWvS%2BztoYGP%2BddjZCrVC3t1lISpSx4XfkMdROId%2F%2Bma8aL7lqgNc70chnmntAs2wOMSSM0YYGWP2C29lZ2mMpNRWaoQPWnaHUQ3IQd2l7eggQ98q4FxV4TtGCEp8MfMT18u7HXRsvoWiVK8rYxqKPk4e6FluyjwrprAOiVAJgobhk6WS6SkySYhB1AWg&X-Amz-Signature=2a74abf383a8afaf3c3670d4c896275fac00977e228653292a93ee7f297482de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

