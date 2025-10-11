---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFO2B5FJ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQCNCH4KBfgYDoHuoVnL164xxk6Fn9fJMSFA3rPd7wqIEQIhAMRQwBJUXw4367Crd8vzn8pcScOMXsr4BiFvqBainj2SKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwsOGqC6nOHBcvgZlwq3AN3gJ0it9FcEOOz6Y4LBCdqWcScD5J%2Fnhoxpbu8Iz3BrIO8NLEmXatpOOmPgbXscXKK3dQvR1W%2FN%2FAZvBziXgnBjwBppb2%2Bvk9MTPyYkfVjqlm78JkkVjyb338wML7xkamcKQ9Q0nkgI%2ByMkW1I5HG%2BqJL%2Bk3TXCqu7bOaATZ%2FiRCgUYwNI4o7F339mRGGLrlJAogifGq8L38zcLabgx5qKWnouyMdbb%2Fn0d97JtvK0yevu74JTYqe1Ejsw5YII1Q%2BWwp97kfNkLm%2Ft82eA2C3KUirV186QgREb7bZbPtx7LFwmBaJCmpharPDbfQBKYMDjwziyVWr09nZBy0%2FaE%2FTIddbFZ%2BXtko4O7MqP51CRMbhUUCkImPToTYOzuc05h3zXc%2B5KwSwdehEoZUnNOEtP9jJ0fL5N%2F3S6%2FtWHJe1lFpdEwQmH7cyXeGX8xU4eHft9lTePhpbiFXysfgT7SFVAaVQ89qNd1hJCduM%2FW3cH4yF9BQTZF7C6C%2BxyNyRcOe7emBF5qgArTrwvxqgd9q8sJ5TXy7%2BAi6Ww2nibs2UNa5eomKHLNEYyxsEBhtG%2Bqyn3hHVSbsafOVjLs9q9WW2SISnzT3rs9figQVsn7TxYXph1CLb1FU%2BvCXA%2BMDC9n6bHBjqkAdfE6N%2BK2zT75ebaEaQJrx7rC%2FWSspFnx15cA0SJQlfJ0QtRfC3EYb7nHh3MPnhfZ%2BKK2qw5SjyvPi2ptlrL8xGQeE8tYFDbb56zwEhTyfuVgIKcAcT%2B2n%2B%2BWhehs8y3yP2Hn4eVuFb8aNxSRoCKjxrHfDiwqksgMebgh5rP2s0hqv4QbE9gvOs4KA4rUH9ctJ32ilhgN4222hcCGXXF8%2FSHcWP%2B&X-Amz-Signature=fb195fd38b4258c80f993f691b5468e6bd58e3ccd762aa71fb4c117157cf562e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

