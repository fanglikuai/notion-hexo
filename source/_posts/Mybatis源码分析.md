---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKYLH5U7%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T130103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIQDKtIE2nDcuxdOI6ogLCAOsBTndUR0dSXO5hWmQNAx8vAIgfqbXYK59RZKcY0UOS%2BnyE267XvgXs5TkQu9xWdOClgwqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW%2B3XwiZOsK70CKOSrcA8H8VD1VibYHSIh419U3IYpq27HfZSPuiqMTnpokWdPTB4yHlXCwsATR8LqvyBu7nk%2BKnFAksfHd3iTdxXO2Igv88zPF7kss97IL47MiJMpIcdwV4j1080DO4r%2BC5dCoy%2BLTlyviRgjJ5%2FolSatUq68tbGxzDixH3FyvDb7Kagvcgw28Unse%2B6aQ4rDZoT%2BxYFWjHcIEQyoAkXORq%2Be7xBk6lrbZMJa1p3seXuIRWBfOr%2BxDrXKxQimhlWG7lmsBH2OtsnK37192r2dNaQ%2Fzevvp9yH1CAfbotg6dt1yibcNXOBk9f9uhPjig9qgiDmfmMscIcRkRxiOWxXug6XBumm052F%2FzMQKZGYXVowJLpaKhj48PIbvNckganbY9y1NT4EBaXgEmxeGguZRL96gw%2BTnjL9s8v6HSJUbJssNzGjE%2FAyPmyuv4yN6MD6Qvgw0TEQ6SVgowC8eZ1VHZklMdVKwElcsnG%2F%2FhPz9JApI8rUbg644gGrgsJKwrTYw9pK0c%2Fjc0v%2Bxc3HXR9crSIgtBIy4MFe5X43zNT9UByyMHnqtbRVHqRmxFsBxUiVINV%2BFzVv2Zq%2FrkrgW8nKVlFpVFnPcyHficgDb4JxOdfPzWpH14pxVjCQIwO2Hur1UMJTVqsYGOqUB0KENV4u7fW3Bswh323xaHte4cTq%2FRJDz19TGHzLt3bN3JpjV2UN1uKYuPNfFUOm60vCq%2F%2Bw777MrUnK%2BqDYjP6v1Ti1Pi%2BU7pWa8P2s44QRRzzyIieeDYEKOVfb8RaWvEb6aQzTX0lTtIdL6THlAQPBCUPnSFTPmjBLWo3LrfpofzGaIFtqCN6p27O2yfgyTd%2BxP4s0J3ui%2BywdZGgqyXbSMG8xJ&X-Amz-Signature=6eecdce586ba38d512b61bcdc96654deece489b1887ab146d6815c4590f38968&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

