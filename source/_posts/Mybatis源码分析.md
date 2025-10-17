---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6K6MPPR%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJGMEQCIGInNWSyW7qr6KRtdOtNexhTWrPwktpMyJsHvHvj2HGAAiAn7BJu4sakucRPYhvsKVifVN5NE2Rc4pylZxuhrsccayqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMFMOfTG%2FGLvnafB4NKtwDo5BqB7PS49HFd2oZZtfTSlWAH1LEn5VRJEHKQPCfRwKWapdEquiJjT26n2bInBYGIRG5R7gtyiLOxs1WkdeKB05PoVeUYs03ejVyhRTJimnd9aH7Musienxg%2BAArCiu71kxNg7TjP%2BQZL6AFYLR%2FeQM%2FKoZvLAScvjcW0mkFX8GiTtMe6iHtdMGl5OKmdECmh%2FRJCeOQ0F5dFryHrnrh%2BcB8Nqf1hFd0jwSCz0CdDjAYOUCJ5IpEhvUoa85Gna7v%2BGw8XkV4vG%2FmK14oeVDR%2B%2B8ieMVOUTs7b5WFGJ7zkg5MT5Dm3eIaLf1GfBi3ojwlUcioN8X3Rb%2BekqVCbL4q%2FDhnLGDKICT6OzK4Rat5%2FaBlXLvdgDj%2BOWWpYOOMKGa1CKpY3uhl2BCzgnWBiKTCGQhPZ49lWn4LdhdOl%2FMPmnFWAuV0T4plELpnliE48bYd5G%2BYBpp%2B3iV7ZMz0U%2F7RNcio4tC7kxwQPJ0RpSZpfDXvePP4MG60oN7iyEPyHXFfnFWfLpDeXnH6yf6Omuw1MJXvQG%2BN6mRh1udOH57DfbDo%2FYi2oDmaRga1l6wmwnZxEBqJT%2BSUXbqFAiJ0JZl27v8FJS8Ec1PtHbb5OvGIoGaz1bSmKA%2Bgy9tVjgkwhvjKxwY6pgGKhl%2BBI0vDR5HaOVSdS40ScIt%2BtcHJ%2FhiHz0a4RzBWFTnZPlv%2B6R3yKiep0TlJVSGntfNstSScPR2KafzhTELZJakuJRW1VYy4VUBbOEJXXvKNYsms%2BFmlnocxpSKMQu4ylVcD8mow0UF8bLsYJ9wsal4mRF4i2Uhl4ngdFKtGatzBRUsnSAjvr3FHGePAH%2BEqsVdmuJv10wo%2F1sGuovAKf5WXNd8v&X-Amz-Signature=e17f27837b093f576e4c9f7efa194054e330b11547458793a9daf3386962a93b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

