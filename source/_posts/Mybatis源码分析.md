---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FJWHFPJ%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA1AUIeHK9oeLVpJFgKVHgeXbpGbqYOMmdhMg9y5FviFAiB%2FnUowcuue1ctK5DGDYpuuoukjy0QtzEPmXhKWRyVAeCr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMcV4dPrwz8tl%2F%2BGLgKtwDJZ5A0DVwwW4RycvCHAKxiIir%2Bhm9uTx6a6GPNnzaa7qXELmcf%2FulEiE8fqc8fFzmfokT%2F93xqZhqag77XyDm4B3iydxk7RZTrza%2FKaEy23tA2ZN5zqVAdlatOtxPFY%2FZzFck53OFebXRRdXlA3hg%2FOSsKhmGsjn81qejK2viunN1fFd2eMXHq6DOVnoH77DvsMmfOHLBLT%2BRsb6jjHFINSNw0Nu3RFIQTGf3l9r0b4ll6chhBFvW5y9Ai0imszoic%2FjWEHnxr3L2kIdudbHkphZI1HCwwQWWplzHAn7tZrn34r9MKCTt818%2B%2B8E8FHXuW0gERAGbZITr02TQcfc6yxZCPuQ4QVJflQeItWxjBpHEbRfWnhUHxnYERCefG9caR8g%2FmO7i0o7LmEm103CcsBTCKVok0wswPKNRSEbB7jDbhpeQ3O3mxE3n%2FLIYvC5XEv4YzOIpInBI5pNUElkEO8xcRPJXYXaE%2FK5%2FD2RD9eHwlW%2BQosgJVQ1jM5Of0DAQ3t9ZdvSh1jbIGQzwilqjE6w5pA07Y8nPh6beYolAh4rxBkp1mV1qHpwub3MH%2BDZaR3H8CttlkJ7S1YsYdKa6VeEbA0iRuGcUEA8wrZL%2BvKHKmvSQ6m0rISbZaysw9eCDxwY6pgEJPDEO6OE3aYtAwipyA7qTby0o8pPzDUajyDeeZaQy6%2FyIcURjbhc7xhgg34Xr8nV4yyVMsbNqg8MRVSZEdb96eQRQ30z%2FvFVfmKEAqYwg2f6CqSiwyWXfoD59bw%2BckGuMyTCE8tZjy8LVprCz3Sd1PaNUofPrcD2KDjwpEpCMLEHvW6a7nqFVyYsZPtlNxXsdSf7JM0L%2Fm5gKhmhuVKbINQ6i3qWX&X-Amz-Signature=ddbafe29b53b655a0374b07e46e4b74468fc1241aca141eaffd7620eeb5c4e0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

