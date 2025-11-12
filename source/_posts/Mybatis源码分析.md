---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646DY47KP%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T140039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDRkgrzk8JV5lJ5MFfSzQ30Q4nKax%2BK57YdcsAArm4FyAIgfN7jBY6sFV7TuiaMGTcZ9RJhCw73cZ1QiE6Gcf0A%2BDkq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDH7CQ7rv9fyZLy7oDyrcA%2BJvbZ2%2F61aEWZx34%2F1qvIAphOXSW1B0p5dN%2FDH7d1WlpD5WWz7LiYcoCvswQkCY4pnhjMkMVn5VotUP9jsvWpfY3nsOYFbS1aP%2B6my8gLRs3d2zpLqH1XlW6i%2BX3JwR0gK%2F0LwLVz3ghPH%2BDMV1KylGfFXPrmjZHsMrFF34WsM5Rmoxb2k2BNjtVQCE%2FvKkGQL6UtiIlCU%2FmZksqlSMp%2By%2FR6SRGSON%2FU%2F9vA5g5DiHCg%2BjOf6YGeZl9dyG%2F%2FK6ck0l71yu0UABaJCnOc%2FEeGahjCvkwD%2FR0Bwl7dqBHlwLkDDvTMyv7i%2F26DDvfmyOG7atLRSeVCKzI%2F1M1z%2F%2F%2BqY24zD5wg0XWV20Hpyi9kK7mWX6puB2Z2T8oOQUo3M%2F0PF8oKG1YvpaksvPo1Jv6om%2BdlFIQDUkjETHHahPN30qFyKKJLcreFdbThnaOcgtXNjWfMev3uxUHG0UQaySsySDwEUW7p2dpP%2F%2FEimZYbJS6j6DmJlEX5bl%2BeVRWF3%2FjaS0rQd2e9d5laDNxCR97hU%2F9oElKckD1z%2FXztyuU1Arog3tNVWghJhb%2BXm%2FvHssqKYLqe1QKm%2BB9YxbbnqrhSGkOfb1j1SwUt9Lcmryn8HE6D53Mbl4ouiNODgmMIeC0sgGOqUBYEu1ErMclZO4ZDm4%2BEAb%2FUnQawlc3MrkF25%2FqQmN6SgFYbLGIX96bG2PaSuSfKyFF1uhJdTTM1%2FxA6xIvH6a1HCOTnE8ZZp%2FM%2FEdMExUdCc4KHNSAxGt6S5hX6rdand3pvOHx2W5aO2rUmiF7JBpNE8xZXi406z1Ob4rTEtJvsMEwTe%2BqhZ2mJOOnFdr5USOOoAxgHhLjqyFjfa6uYiZmQI3GV2H&X-Amz-Signature=eb559ba95eb4ac81dbe6e7cbbfb57bb99db83faff59487ce70ad1a73ddf7f441&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

