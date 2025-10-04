---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSWQ5O56%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsmHmYDsWfszG9FaaA1IC1Nt0dXZ%2FhnPWqSoMNhXDaTwIhAI6ednnCFXWwQeuXW10lqWslg%2BtpzvMlnZQ5aWc3xxFJKv8DCGEQABoMNjM3NDIzMTgzODA1IgzPJd3gc4WvGcYA43Eq3AMeJypH7yWLinYDIOhhOgHERHwxnzTY%2F2xRdyvMaS0Zmrg52g6b4SunQDCHLdnm7N6tRcaxaWQkcA5aQdTPwM8U91OzvIH13MEJNjb%2F3ubDTE7yyztyWwSUkBm3%2BvibBJV%2BlRPS%2Bh5thXScuP%2BzsH0esOX3VcvSS6utHRTmDVjkva8WuTHjbCYGUEKOa8pBdd09xGUpmUp6i1cRJMPXqTl3%2FY4qXzikyapwCQKQ5Of9pAQnSU5tZi5zcWVEyIciUxr0kX4mFUdhAaYa%2Bh52f7fZtCMAK1yik1HCqykw5MgpiN8kI02CNnDeU2VVbtcpQYy6sUl8%2BLBPb5NqJqKWFL6955%2Bd6SxaltxgYeeEwWRuxagi%2FY%2BC0MSRBGayLo%2BOeoUXJPZ0hKU8O8SZ7B2vte24roY9vgTkKnApK%2BkCvJsj4QhEEF729qti6NZbpS5l6X1yCNnR0KRZdnMwI5VhfuGoYsqAmWO2ZWVDPsysn8InrY4H6jhsqjMY78EDoLmBUrkGOc4XHWBolN4UNk2mTytT4xgFQf%2Bj2JT4qrGAD%2BE2J5tbxUgDj0ohnFp74GprNOPgwjaBbfDLF25yRndXmMMRGMk70BUpOQxzmqltjV99BZhJiT78mQLvnvcycTC4kIXHBjqkASPa0URqAJv0Xgy%2FjOOzBA4VFFh%2BIbf3tpplv4zAjsH82Kl0%2FsBElm1caN87IEtb60FLxC5F%2FKrOCvygUlL%2FoCsH9DLJF2xlRs%2BOeF26OuDoGgyXDdoFfxJGbgNhUSnvqx44lKNH56ER82YNGhQ5UdRIwe1O%2BIt438FnIq34oeFdmBKmje6%2BTHk%2Fn9JxXm%2FWuul1X%2BH4b29ByHcy%2BbxeKEUGogsA&X-Amz-Signature=d25de927c49f262a24bcfb612002b9a75ab20f58ef8a482452e0e0bd4dc4e00f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

