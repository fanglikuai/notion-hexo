---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRZJBB42%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCz0%2FDdC29sNZ2dp4zn%2Fz%2FUKtQU91xNvE1IAB32xwOqgQIgHs7XjnGqqzTvgurUd2K7RlWuVWBRPKhbKTGjpOitXskqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHZWCPQevMfmX8CEVyrcA2wSHbCVcy5BhK8QVY0Wj921W6zM3Bk041KRRkL4kU8aMtXLtXzpjRHu1VufLRRjT3nQzi2G1PuBQO3IPngOW9yRDpVLS1hvouBgiMNxRo9L43%2BeObnll8S%2F4DVxcYjB%2BXyMRNG0ey6u9WMe2oKAqrlBKmgIPyKuVBxfQLHu04JmaP9Hfr%2FEhs2QZnGRlsp3D9QDraGTQixMlYlK7nn1uqcR8N6uWHXGs2i4krvt1d3SWlQSIjWW1Z7Oixw6w%2BEaqsgglGyEXsE9zgj1PkzEchoSakTxRZn%2FUBqwCpqMeOUf%2F0QmPqQUn8YvdBIya%2Bb3K0vMGaB%2BQJ3jDRL5i%2BZJIJrcFUgCO7KKoCeCNIqvfpxv%2BtQ5SWJdOUAfoHO1Bz%2BJC8AuXR40fh1xbOQ49yXzMV9%2BeB19toiELF7DmF8NVvl7lBJDyYGF8BOdpEjnXMKSWAve68B4Vani9eAG9QAA9JU%2Bmfs73WNPbqPReME74PH1WS%2F8D5yOWAdPj2x29wOO4yOzv5wr42mk0IP1gKR0amCCHB5OLP4lct2bIoI57urLqT%2B%2F9t6IMO6fSeGuKtLQX2EnWdLdriS4dCC99ckZVBgIxl71POt26fv%2BQof5ULkjMApYmFcWQBZ0rSW3MILYwMcGOqUBN3fmJYqgUDFUljrGs3LbQokZOaFEM3EwHxLKvCTrrDKGt7ClwWZyasgHPyFubwUaLZ%2FZjtdrnfZ8cVgHWpk7cFZUD8Ipq%2FJ%2BRNKzHYyrrLH8EeTM02zUfET%2B%2BXYrixbUKNXLObCqWLFR0SRvgysvFfu3Kxkf0TlGTp4F%2Bq4p4nO4afckCVMTIsksQuijEunRp8g1MBApVGoA0ArEBfwnNj%2FYzxbc&X-Amz-Signature=9eded42a93cd7051ad5d9ecf63d6a037d7d021c706a9b064985857612ad74178&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

