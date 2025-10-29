---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPQCQDMV%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDYlcpR0Ao%2B6FqcWS5SO9nW7ewHGu%2B2nQVDO2yNmXiR6QIhALHA4617M7vyp49NT1VNXm5i%2F0xQADoAw3HtrLqYRO8bKogECNf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyDcIGWkC%2FfuTYXLdoq3APsgvYkrDJhLr0dXZsDgnZ3wGuqfGUWn%2FQHXM3Kx88tPhzTBu344cWlQUKEnF05MfBgJ7DaYd9TJtcrybXJ43OlkbrLbSHdetk1kVclWWrEnh%2B%2F4pkBx7ZaLpjEWv9ZO0fKZByzdtNEM1kEGcWv7kp%2BqPCKViffheGAbF2A8ythevq%2BY1NNhYy%2F4lgur%2BkKBpGcCga6wTxUN5VV0hSwj7E6p9yLjBjl9apPr3Ns5kjh68NPUpsmne1TRwj6qSTYoNgq9MPL25ukjhkUlSFvNlKK%2FirHyKQE5I3l8Li1ymEeBQwIEdpRqEr8fiWTQz4XrYMfy1hrT3IP%2BCVLDd%2F3j8fTUgeTNIq73jKFBygFmUmC0ZD%2FnMU4mnZMDyA6Mdem3K9lG8ZuEFCt6VQPXPzxQdgR0Ja9PVZVv95DxeiumKGg9rf8LG5eyij1%2B6SoZB7SCHcYYUnQ2s0k9QNjK9ptPGSA8CjLtBheY%2FupI%2Btwlvthu6M%2FUJUlQ1OuVccMDd5lQbUR%2B6nRw3AijXKxQju%2B20fA18GZlCvyJ65mpdptMIYkwzti7T80K01VaQw0LtjOcqlX%2FAiKH8B6n%2B5DLBzqxA8CIj8t7e4o3RRxJR%2FyJjUnwuwQrai6W1jq8BMCKDDVrYjIBjqkAYKK%2FCZF2HnhmzqRlodNxxr8uTmQRvNXHogGtEgzn5WtNAK8nu7deaE%2BufXnTjsno8FdeOj56sCS1ffBx0H9Oyg4zlfxRGmrjDagPVxhX%2BKJli4bM4iM8TqDMDNdMaHVjPdr%2BgIcPLR3pD6cfXFvKoC47PsVKYv7CsnGzcCBf3JS0s2ZEeya1IVxOHNKrtIk7v72XDC9IsUpSoT7Yow2ts4SPpAD&X-Amz-Signature=2c3b44a1828a61862fb0324b4d914dfeb20fb9d270515656aaa7f9ebb0dc4c15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

