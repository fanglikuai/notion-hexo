---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWW6V5JM%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T090054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpUOCIDklE9vA3FYSCiDzV8gE%2BGI%2FErRfdBPKMKCGtzgIgFUjOeBXpR8sOS8Lk3%2Beo%2Fkts%2F9Q8aAxb8UwWRbB78J0q%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDLHkC56yH63mu2y74CrcA7x3ldH8iD4YmT0da1l1n72fOXFviRQVB5fX3UEnlGTAnfQkS%2FvMc%2BFznklvZ14xo9Yjr7CWr8KYtPu4jToX2Jf8K%2FaNG23R7D7FseK7Q6wONPXaCFt9xOJCGxRqiGzjbc3DfMN2iu7hXn5qHni6lWcVd8cHkJPEU7Dfzuq4SOUUvI2ZnthmGWjUzdqDrH6QigqyyHJVDiwN3%2BUnuhBw%2FfOJCciMvgzSKlyjZb8Qnze%2B5LYb7af7pRJ0qKFOTTUgWI3d5vEaB%2Bdvfas7%2BqRwtNxGL1M87BcMqrIcfofWrbE0XSx%2Bk0vvJMOD3b7%2FE%2BsWvAx0B600p22Co58BVVbixrHfBV%2BKtZYLG5I22xnKqCXFw4M96jTdIh22GnR5K3su3qqjg8kFYXQw5YFHWMaU0%2BVIuqITFdbm2uSFGbODoee84MWuuNdu3WTd1Hs7w1odQEOerMog8SeOgj8owcso2ZHm6sAnlkmqpC86F7PYffftGJT0CWDQIXJSzhqOUXtdfM1ug8N%2F7UXkzdyv7UlpEZiCj4JM9zdFlnMy6oixy4RrsP19koaoIqXsn7yvqjxRo%2FXaawIPBjO2xBg423ZCzTiQvj08qPrRPqffviFAZhr76wANK%2BUuSwqwHmOLMLfq8ccGOqUBwmAmy6NQ1QK3oCtmmgRvrczRjutISnBwqr%2Fpc0zR81VV8YlCk9Ywb9NsLcAYY5PNorkxiKPsHnnue8HTTZboUFyhOWkiHbe4HLdeYiF1ErKSK76%2FNu7VO1Q39nh4pvSHcgjA5LdLqzi4HwZarXfbrsMZi0Fg16oFUcPzW8lCmoKy4CsF5dmA9AEguUwIsr9jkjjwE7wvfRE2pRHKpfn7AZSsxBcB&X-Amz-Signature=8f7cfa199430a416b1a480c41fd82e832df685d91d2a51c382be982e339a67c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

