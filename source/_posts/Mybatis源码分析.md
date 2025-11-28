---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QYYDIWM%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T110054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHNbAVATw7siWZll%2FuqpKHqHcKy4c%2BvCbE7ay%2BjG%2FsiQIhAIBk9HXtc6wfmNUKenHVDSh6XejY7BfeHViUdjqa3yGeKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxzcZJXOtSK4qmeTcgq3AOtYmnKWIvAQ4xufGhIFu6LlOD46fCVcumLNFtcjBLEOWDEtx4Z44UJxpsu3HNYLil%2BfNK5FUSGrohAyfcyU2VD4Bnb2he0IYlmV7Jt1reoySZ7e0k%2F7K3zO5458yA%2BoEQxgQZ68FxV34l420ff9EkIwhpIFz13YZv6jzC0LKEUnZVdbFE%2FxmUAnvnxCK2xvRhnJ5GjqzRqK%2FatCERDersxKxI7nCtYykOU74I56ElALU7Q2vpWmkQx%2FmFd%2F9559UValqIl92HFQYPmiL5VYv%2BsEKqBlZK24Rh%2BUyKe54ryOegwHNSQICW31Uz19x33cSlQr0fx%2B5c0ohXxk0sKsRO83pLM9rppBcmjezJTWUJ7CmNkPPO09lcOPSgqMEZ2vOTnpdtX8e67aFA4XVYAqfTGH9b6R29V%2BeFkzujsMnVnXKvpVuWF8Ic1oti0ZQxAeAWM3J%2BkIBrmSMepDNgzFdV9mgkIyEe7dWPVKwflvIiHM55pa62SGtFsJrdmnh3otk33gnNPzzO%2FQl%2B%2FERCt%2FD2J7MD36Nq8P3Dn9geR4Tsre%2FbtIbSW3JL9jH3utEUnfjpVNJoY6vGyYgc%2Bzpcfc%2FcbkanAFIPlyrdcmUT%2FK377HAhNvbLFxqAU4GfeWzCe4KXJBjqkAamjKqwnUHa5i3%2B2G2DVmtDU%2F67sWbm%2B6yKlMMCloDQ9UVh5a9xYUm8HnvLl5UoqPNPmnf%2FS5wOGBfRgaSNTw%2BN%2FzB6eueK%2BSb%2B0ueZ8LLveChGHxoork%2Fg274Vv3vKDgp4xfRFI1gLtnqbmi1wsqKSJ6duiSYRH7vCI5Mlq5DxmT6bcFZshSQApY3YNCC7c73d9lbg2ZYLFYr1tYM6WfQ9BFYrj&X-Amz-Signature=9917da84034718b165f17bd3413a32b55f5d57d34af2cdeb8bef25595e32d68a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

