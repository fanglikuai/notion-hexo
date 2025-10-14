---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZRY7JRV%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICbKvsBr6to503fZI%2BZqX389N2TiWgzBa43I8V4UZJ25AiAUzC%2BEeh%2FJkf0l3WT85ITqgbs1peD7vb3OlB0WY%2BPTvir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfAtzrDO8QNXJZ9SyKtwDUKD2vE%2FzFAxbO3R5lAS1FLzkop%2BqdGKytEHsFzdqKTx9qb90S1ZJLRNBIzja7Tu0zxUYQ4NbhZ7m1xKaiY06BomLaBgfKMQxccggEmOKtM59Eqg%2FH89M5XDzoL1oFg6peazVMVgesubUVoJWpawMmodzNanOexO%2BM1iopfdbw%2FcpMiCQ3ZUbNuQtynqCFEHO2Ctn1dYnDCDHIdwOXtUvm0hCE%2BRSGt1LE5s0Bht2L4hS4E5Qnrg5HUySOJ5MR%2BZxg6NNN7CWBDUo1yF7sdYjtxPgTQ%2B7GxrsHSp%2B3GCEaSp6i7ETwxhCkSHF0t6xg%2Fe0uFabNDlxA2ItN9dYF6MSl61yJaudlalDjL3AEBtPe0%2BMvF6q1Go9QB%2FebyAcqjjxYWvNxLsFSI7JI0iUeCo%2BIjAvcFUEennZDa4YkrqVpaalJCy4I%2BKqKwIDf0MQBFfeN%2BUhSja7aaXBuzjv7n3CsI0w%2FR%2FOtNnXszIYuxWToJ6ezUm8TaL2bG8UNdvcMX%2ByNxlI1o%2B0t767lMxN29ZWH0yzln8iqaVt7X02sFi80v8872su9MIASKXqyIFSuIC1FK26s%2FIJKxi9foMBeHYEQnLdyYG1VkpNXFaxz%2BWf8Sj3wudxSijYmMl1mRgwptm5xwY6pgEa5Ly9LUrSIKlx3TD5wsZyBJGGgLtisdVukUr9OJRchS8Sg0jS4zdJXofmLsMBI2nHHbxUyo%2FNtFNYaqhW72O6sUIAd%2FdVA%2Fz9mzbb%2Fl4nQoCnXNHeTXz7N8rKM5rOjcLjR%2Bi2VjuBxBgijzW96OZrZynREZZrW%2BZuGhZZOmSTzyqbO6WLG7MO2GT5PQN0uaxfRh%2FKGkMuRJ6NsHNO%2Fp7%2F21IFftId&X-Amz-Signature=0263824d2d0cdde5ed6de4da5a0abdba858c719ed84ffe848016f444afe68deb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

