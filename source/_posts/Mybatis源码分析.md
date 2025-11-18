---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBDADZVV%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE2PVEvxDPlpMMLc47NKjlYVpHMaiZeAexLsrht3rykJAiBB%2FtrDxnO6LhSFE97UClTSHEqgAdZj3MP%2BpFL5L6T6qCqIBAjC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEE1nBQE7hx8mfPyqKtwDHxPCnEGUt1XbH%2FVxwiG7CWQepxDYqWw7VZZzvvEDkv80xL7m2U5PJRzV%2F09DrsPjZidJUWLRo00SAuRv8HFIgfZu%2FZtGeIofIwMtuK5AtA5ugVn%2BvQ7BK60O8VS%2FN4JYN8jfQTQIojuI0CazHlivdhGj2umBcuKhO%2FwkU0kIqSPN7CnanVU8qp6SXT0UUEjX4Ckv7EeScwktRxu4AWFKf6xWvvHXgjsv7A1Rz9mrEQ%2BqG0GMTY7fP783jNq45aafqy%2FrXIQRtAVyqVHLhIbwD3L5WsBJM9xWtOYqMj%2Foa%2BFK0kL4Y%2BESc6XI08E9QF5bMWZpwyOChayuQz3IuxUgUYUJMJCgKYWEEItUal7LJfc6a95U%2BGtuTuW1ApUl4F3pclDeELxHskLqw%2BafcSUYmp10omzRCcPkGBPLZt26noTCuxKnghgF1u%2FrDNK5p%2BcH1lczaTcTUtTprurnOlYr2%2BUafhD%2BHnS5fFhidjdt%2FCX%2BA%2FxSw7X5Hs0wlPsp%2Fgd14l%2B9VeRR%2Ba0BLCHMTa6ug4u%2BOF2XiU9Gigvxr%2FRvAck7srASoSmrb7u1hDEb2nOlT250iP4V1qXZXedLPDk5ECfpQeQeHrwK2CUIUqcVOuImm24WuDwfU1NUNhYw0eXwyAY6pgGEoaV8aWs0NQzp8uwDYZWpXuD%2Ba%2BzRKoW8LoCLcjUeIoNLwrctOq%2B1ZEoqePnEn8gnENWaFEsdMQ1AE1OUeXTJxI6G%2FEqgRU9QavpXDRMM%2FPr%2Bi7CJPs1W6hxuzzj37r7acWU8eZWKZ8xHJcQaIcuV2ozg%2Fz54eei4CJBM9KU2BVmjeHzGmLl%2FSHx4%2Ff5yTgZ5iSVV95el%2BOCQgR7C6eQluXRyV%2Fbo&X-Amz-Signature=ea650bab73c0585719f21fabfa0fd9f6fcf549989c10c5135897a51177666ea7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

