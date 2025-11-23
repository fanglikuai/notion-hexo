---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2NDRGDW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJGMEQCIFWICmlE7rvwrX2fYz7oBzwvKX%2BxiS5wf1n%2FNoknEUfjAiBQPr4VYhmCLPVvRWHD7WbGroNWYNM%2B8XaP7sMa9N%2BI0yr%2FAwhAEAAaDDYzNzQyMzE4MzgwNSIMpNUYlrpF7EflTXpfKtwDpfRYeNfnmlzvdnEhma1KJTxeIbXgWUGUmf92eyYMHhCYlpiFfUYbHuGeGojG5IqN2Vgp3krZ7jHpJ%2FKje22QyZpv8pDVr5GKl5yCK6fheCkGKtBGeUWaNxSwQ76B8EV2SoMfeeNcpXPejaFUMdMjLjijMik3WHCKzWpU0Mqzevpwyky9NaCirUZFBTPaohwl8jvBVwE%2F4kCiuGPzDdxxOOQpgK5jRrT3%2FV5T%2FXFZXujNXjWOUKGzk0arF%2BK6VWqVno43oBZuT9j0CPPUwJlcxxXvEbgc6P%2BlMPLopYBQMR1ozmoQ6T0E8%2BiDK%2BQghs0c1gggyPKsYy0RhPGLBsb51HMns4KA6HoDuDlJER2dgn56iLR7Wde9Bxp5peCLtaj%2F9bUWmHGYLUe2ng2pP8AuAHp8ZzZP5OVKvqrHYh%2BeQO79LYO9DJ66ZU83inT%2BzG%2BixIsY5TZ3tgv6f9e9nr7x0KHSX8FXAswc%2BUFXC4Gm%2FgpYPtOSGxRkxRlo%2BznrFJWAFv5CksXK4fzhZrgqEGtGA69mik6ZG2q%2BCZSTxQdhNiiI4yTemjjv4KDHzMhmli%2BudaafzuecZJCYipURC3aIwzXSoi1bWRy%2FDWOIrJo%2Fa01GPyHgcBqMto97QXgw%2FLqMyQY6pgG%2BEZuC9kSxkRXaQxoP1lhGk%2BcRBWdR8CWb%2B9W3ID7HMfHUhigOpvM4%2BJlk%2BR1DE6%2FC3zZtWkUex3cDOqZKOI%2FCiYU5MRNeQWPhJlcIrTdJZfa3hx1Za5ZlMF%2FWjSdzqAyqdSa12NHswYzc9HPiwlzGKrGH4qemp9eZHwBikFf5I2S2ORlhsxAQtsrMkCKyUtoH5iXd2LcEfuH1jpQyLi%2BKYe62fzF0&X-Amz-Signature=bea6aea7be4e2d4c08010cee051e515f6817faf3e84fd38112e86202467a68ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

