---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FY7ESTW%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T090708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQCGifxaSxWjXL%2FyU5AGT5I2wzCLFraG8EvbVBDCLl6cyQIgcCTxCrif9sW%2Bu6pGiixoEOA5ceF2XNvYnlX1vJzYE1EqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeoMNueR%2FnrfVKGBSrcA0I6buwmVPklhy47dTqWJS7nNmVC9z8Z46jpaR0Co8YxjHXEt4D7PX4rPyjpWcWmZByDYRn2ALUqUl0g4O6yHepjtISZ%2FVuppXXR7lCyIaj%2Bu0s3vjWdWijZSlJv%2FabQUdNj%2BDPTgIEUYGwgU%2FZvWanicuW6nmtXjQf8S9G3Cfyb0gldbZgnYKFQakOJrzRBqyfGeWCVfjttUuRriGWUuIU6wLadlAOaGNyhJmQUQQ6DD0LhY2eB9g3xpg86dJ9ItQAdbMT2lHriT9yPgLm7DlcTOVlRjYkDZwfNnhnyeczNT16FMZ6iewiRIejeXJqDLvaNquiuYKJtRUPHfFQC4gumYlBYedJzHA4XhAy%2Fqb%2B3hM0PHnGt9jREUQa2NZCGOnSyTSwptO4EoC52ylbK7%2FNAh6Um7YZ444StZXbZmRUYFgeeshd1hcE%2Fpmp1kksmiD%2FwgHwkb7Hg8ZoJkUpkJHhuflmLZpTYyIRwCQeiU191gO72ZP8c61GbxXPAnslxqxyG91iFgquLII9WauSbHzJiZws51v4qckqu%2B%2FF1otT02gS%2BDZgzY1tAS2lrJpd3VWURbGPWWOZ5BH7uywVTakwai4IiuqYtCQMh9BQVrnHyXC6RCNEykAFWmAnRMJK218cGOqUBP0VODZWzoPmdIN%2B%2FVdrVSSamfVOCHyQgDuB9x68EZB5lM8KW3Sj3WbbW0PRbaEDWiRpQzPQ9wS%2F6FzZop81k2tqMVUcWmILsCIIwlqq5AuTabCFVLbW9sQY5QNFoYoYCgZYd4NbIv4j1BoRWde5SzpmwhIZcF5VQYQzq0DTvjHBxggjqwpP8iaqE1KvoZ2e02e2g60wpiU7POQ7g8rIgj%2BUWLg%2B4&X-Amz-Signature=9ffda5671880d16143826ecb14f2e3e16002b963c780670c0393836a1b43339e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

