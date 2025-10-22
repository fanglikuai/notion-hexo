---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZYYGQF7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T060139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDeeZMPUvh7EDWZlctb9rVrhzQo2yVVa2r5o1a6Zrr97wIhAKNYLnOcjeCGs%2F9HpPZm7QBGYC3N9p%2B6KHt6ekTkFXTgKv8DCCMQABoMNjM3NDIzMTgzODA1IgyRzb5L%2BIbC8%2BTtuPwq3APmIYXXo44R%2F%2BPBe4YDzUJFJRxMpC1K76ugLDY4B83Y0btvN2IyJLDbmBNN3hbVvmHWbYzifPGcLRW5Khe1BNtkguOIQO2feN4bbvnLjeXbLZVStdMBngAo5XshjuPiHgWKCZIJky7IUCYLCKvcNjYmxplAjCYmQ8N%2BNMMt0rg0hCeLLfKArtzSWboTRk86UaoPSFV%2BP8WTGivlE3qe6evGJbGINJkBsc8P3LICv6ZX6STE5FmWrMOBZW3cM8hTMVo2OqPL7SHP30GlZbgT6wS27pK2bP5M%2BGMg%2FvFkaPfU4Qz8s%2BTkqdqsBcZX9qWojb4tCY8Z4neFnOCxDhw4upovJpRoUtVvD6e%2FQh9BQWG5tZanwSaQOY5N05RfCY5BemWTQ2YOkhmYBBc0n8TFnKtE0FODGZHGQRwy3WH7u8selSD6AWKhQqvFnIP4NlFaqfmm1UBRF4usHNRDyfiwCEHKyMdLgdW5afs3H8tfzVemLgcK6ylO6cUBMhiA8XZN2P3nEXXM8ZyJJ%2BDt2lGF9WZHi8WwsWiSmiR7IeT4S4eZsxeofL5aQN1eIO3o2V%2Byzks%2Fe2IKsGbAopsXnL6LwrXR%2BKiX4mYxNqldmw8G3kM4T%2FR8kYIPPzOpFdWHqzDQ5%2BDHBjqkAX%2FLCb%2BSp%2BAb%2Bpj6z5osPMuy4oyuoDW9jhENbnCsPeIasw8Od2h17l7v5H9%2FeCCWwdCdE02FRlMQaZ0%2BDm0Gh078RKgB4xnPdi3M%2BDJxWGnzleluX04csYYkfyo%2BYlARHACsjreShKBrybJbG6zGVRBtQJGNa4rDA0oqvsBelzllxAadYpEwUWBkIfwfOybuRd0dT1OYPfLqX7BVG%2Fu8eRPymzc%2F&X-Amz-Signature=3c5ebfae5f0a79755e42030a23bc866dd275a2beb2376ceff5f4f3e31a19fddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

