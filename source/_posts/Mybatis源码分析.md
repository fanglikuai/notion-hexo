---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOMH4S7B%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQCAO7NN%2B7FD4z0ud14Wrpn%2FA7od2EaYxmkJv1NJ9aCKigIhAPWJQceeSc%2Bs8eBUVhl7IQNaKJfnkRMPHnPgdeGg%2BC%2FfKogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxL7O0HNJ285oAq5aQq3AMuWZOHONKPdgp6Q%2Bb1%2BaYqOftjUxkQMg5K1is%2BbeXwIejas%2FGzid6dSzQlnTMTuPBXZw%2FJchK%2FF60hc%2BpSdPcXGp2lIDO8AAhe%2FtNKldhCWHeGx0I35pWEGuVuBtuIa4U0O%2BNKYD9WKplk8EnheTxyQe6YLcvJuT8162gTRPZmQT4kq6U%2FyvOuJMXsNdMU5A9KXGcVmnkD1bpbCNYRUMxUZ0hXcgjNO677OpFLmwYs8DweJphpt06wu3uP9Uwq4bX11CuAbATgNfwSqZUX6yeYKmurSDWxi%2BmGYrN4VnP%2BQ4EMtpoDlghMIFTyxzdozSFdNBus2N8JM%2BZ%2B%2Bh2q%2BizcSUAFBarrcCck4M0VCui34c%2FRwnmEVK2a%2FTQSTMNJe0kpouc4EEPDigGqc8%2BxC5b8Hz%2FmTHmBaYXlMpyL4VAYDvB%2BGs6gYPecxmWQnywg%2BnOTtXhPIXarrPT6h4%2F9EMt2F3%2BoDbrlOStVqo1YY9OfTXt8mOUoXMLzgfuGJ%2BJKCfcvXG0Bnygz%2FyoOgrXcA9BRildXFOw2g6PwkC0pNqkjGMsp35NpqXYF%2BPFjCL0weade%2FjK%2BzWUA3h0ve0Wyo%2Bdy6CsIk4XNzp%2FQRCKNa0T3zYOmopKk2cAgaQVkBjDNoJXHBjqkARJL2WaWuLPohZnNLMLr8AW09SN2W1%2F2M7prGw4nnA%2Fvt2aERPd7eBdKYg3LKz%2FqLcVtnPrvk0jrdHLnSY%2FvXEnu9WitZJXSeM4%2B%2Fgqoy%2FKd7JW9nXVvjiNMtHp5tM56mPRSltuIeubupNEyqhcAz7xJwR3CzOkGaJ5awwlmSMRSNt30cCOrsIJVzNVPYMnO%2FwCahk3hE4t4JEm3flgIAmctfeCq&X-Amz-Signature=9b8edc70c262d1d2078481f04dd7d066f076e16f75494031f3555e84ddac08a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

