---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMKHPWAU%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHb4%2FAetjurL2CTUgK%2BUnRBIKzSELMkpJYgbD58vV%2FuWAiEAlroMO6hBA8v99zzXBwKhCgaVqJMcKvluFK%2BPKPeK5DQq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDH7fmrK3hDeEM4rrUyrcA%2BfS3RFKwn0X8oxDjQ%2BNi%2BuAsYJFhI0AHHwvmpeyTGzOJG0lafy45u2Xj5jPQRg0Qds6R589Qc61RvskYXa7xy122jFta4bDQGTgkivFWfV2tAGE9WfGbpPvgUJF9EA6457IHQJUaQgONuhUFvBOwnwHwQBZGq3eakqnc7EK%2Fm4oYKAPDOuBMHtCKYvZTxYNgfpLszIkIiVFpRCadN0pD%2F9%2FQBVFfW2bQcLYvwWckYZUmS%2Be1a62PqMOE2gv5D13tIjAb6D3hhv7npiPoHNRs4uTSGgmhtYunylJ54NZxr7DBOjXFy8tlWyOzJ5a%2FTI3RVLIWkoT5v42LazcZbseIjprHfesB0WuE4%2BzS03NATxpzLiKQy%2F6T8GD%2Bbhbz3SWtHs5d1DG750n8%2B0V9p3e2EZmbPmDLxIG0GkX0qIZk9gc2zQ3HkPKWMLlvb5H3lZkbeYqXmQAqyD2MvMzkthbdIR8gDn0s%2Bj0B5E5Tt5JcpCVNZ4hHU0w%2F%2B3RyXTDTj%2F7UcBEQnQ%2FHevD9zhi%2FaNBS7zKUUzg0TE9S6Q%2FleTjmdLz01BR8gJrRh0RQi%2BJRSWbZ0R5kfwQVzo6KVqgrN2ZAfETo3lSAM%2BWvqLH2fWrLIw4pt6zrze18M8Rkq6jMOX%2Fk8gGOqUBuP0L9oJAlWBHBQh2UORewMstMBoW5ut4qYH97RJvrMBVjnijBdYrwh1HAQkMBMXOIe6YE0wKg8tm8zbc9ZvQjePyFpKIsm6D1M0Vhfd24rKlf2Gf09YccUSMIC5%2BUY6mAB5HROCtz1CTUmaYzE0xBY%2BvX6%2BJT%2Fc5mj3AGpWkeNpnBxwHgYNBVhpwlBTV%2FW3wyzLb0HR6gfVpHv3IrWFlIo%2FCak8I&X-Amz-Signature=7ad69e63ee35c6f6f2403d43dd6f7348d62bee07673739c41e2d2fc6181bfa68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

