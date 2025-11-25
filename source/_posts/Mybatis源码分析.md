---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFNNDZTF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDkqp1Pu%2BwW7765cdoLpx%2B9SsdAQtPEdtpuwa6sNaPIqAiEA845vFdr%2FFiiiMdupZhh5nosGo%2F0l0U15OGOqIBcAlgYq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDBgwWNGTx3hothQF%2FCrcA0ykWPN0nb7EFcA0KNU3hOsJJpn552x6Vx84zN6fIDdhHhwwFS4B4yU04TmlXE6HmsIdEs%2BsSMSDx6Z87A1MTKjKEMWtsZmvkNnCC0fzEwT18KZvSmWDZbkUtpuIkAnQN6pCn08S3MXjGoJN9S9tZ%2BulVPzPmKplnPQ7GoVuImTffGq4T%2FZ2yoCeB0tWccIEmmNwUROuNBBDpzgyf7m0cLNkOokXRZu5649cgn7V8jGrTHY6q36ot85BwHtMnUq2sSuGlK3buNp2%2BYWmNqdV8rz%2Bu5vJycifFHQuS%2BV5jlJbY2hvNZe7WgOcYQNddXjh7sOxBfl64jHG6gEW1lY67UjnscHlucJd%2FrvvibLjPOSbaOXOmOmzAuQqLQUB0hK3O7ow4pi93HEpIZpBgwIv2exkYCP3m%2FkuBwiBGDstV95OyDEq2Egzg9SBprZvQoIVlKhB0y5IvkDfHsikF7i7A5g2OC2fojYDIy5RKDyiIezd6RGth7vajrnxCi%2BdmqLDNvqgyrGqigXXkQ%2BV2d5bWMx8TbfJ%2BZVnQdwAHhQiyyNUFG7AzFC%2F40%2BEvNpcvuW%2FG0lDZl4Wj%2BxeeaYjyril52HK2W3DrVDOgcERxkUnSrkQ65a8fdMfogUiIuUKMI3jlskGOqUBRm%2FDhDXsevevbZy%2FXZ%2Bp9cNRhccaa%2FomKXJmajxa1l0vndLrmdt4yw7K%2FSD3WatG2UU2AZURyZ5ZsKuJ6IyDX3IghUqd7w%2BXYZZGFOkD72COuwvXeY7mn9MGnegJyymRazZWqz5KngAlCEzshuljajbf4O%2Flo207jQyhRnd7TOy4WWkkruKxDhNiWs%2FDclllR8Y6GmIC6xEByakoKbXQoLri7Pmo&X-Amz-Signature=a1737b6a584a07f7ce613be029d88fcb4fec595e867255681032d6256b878238&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

