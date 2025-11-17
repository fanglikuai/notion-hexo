---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QG76GBAX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU1uZ0IaJEf2MfgKuSfYPIbr%2FE5%2BU2gb7GqHJtzIjAAAiEA6j%2B6IwQntTGjrOpycOB%2F8UEIN%2BUlpBPtUV8PrRiZB20qiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMjmeAPw3MNm9m82circA9WESGs7i0AdEFdPB7nOVTP8CK%2FYhVxU9Cy1EpW9b3%2BAh4AmQKDmYFGoOSADt9u%2BgDmF9ib94%2BYJvhljoSC9GA%2F7ZrvA0KPFm8%2BXQbYWVXaws1B1RfAn0GuRBiO9jh%2BAU0UBuRF0MOu2EwKxZzZpKPnlJbw7LBMNpm0pjtCnkNKJMSwIAoG42YHF59RM9acn48UBBAduhBpT7INmCxGlQEWZ9TNLtlupBiaZSjM21LnQKIDFu6i%2B4xQLK41RaUt7AIW4pQZYyQsnJXlMudfc8hTP2aWu2LGOcAp0uE5oQg4rxfdxxqJxxOZ7IFg4yDAe%2FweoIRPgQvCRIvZpTsEZqpnvdp4KqxFD4X%2FELfFS8DthxhZLn9ywGGjezggLb6UAHHBxnQQyMYb0QTTCDeRJCCzyypK9R5YC3rO53blIa9DCGmdBk77aVy86O7bex3CTI69Ib1xHCthgeJ8%2FvM1pOFtkj3Z6mSlnVDKtl7yHVM2dyURTZRfU2Kih80JT8cCLY9adtIcujXIY9fMRKd8vEgJhDDb9bKOzNdlalw1ZAybORGZ3%2BPxuZgTi4ZbiimGJMoHzxaQrxa3UKw8oL7VK8aZYVvkMS0kbX4HT7j0hMnuL%2BKzfCQPaToMvnp62MKb%2B7cgGOqUBGZNLcPZjl%2B8hviQCQXksfu7f1nHdt8EM%2B2AjC%2BcxdJ7X7RwyvJLbnAYjRAsnWFJiak88gZw5%2F9XXRjOJdY9ojFy%2FqNvRXnv6zAbwbWCP85ohywRIoMxQ%2BNn0Xk9Cuw2HWbbBGwc7TFWaFUqZ%2FxfgusbRc9rTca9Y6%2BgqSDUmKNumZsLXI%2BcOJNrk6zqkNeOYMy0oNoUOWGYqSd5m2OCXw5o3%2FAUS&X-Amz-Signature=6f6d8db7454902e6d9ecefdb9a8a49ccad41bba874c01dac00621c08568d5cf1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

