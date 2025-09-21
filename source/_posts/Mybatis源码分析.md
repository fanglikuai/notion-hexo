---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V76YQ4M5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEMBaONppG%2FPxJvoumXemSgBFjJJ7gIGmIIhH4NSII69AiEA%2Bm3mwU6Oe7Mlf8XRNiqeETF7J5dNOMeCLkl%2FV9dDmqoq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDKoPzYz2ol14n9%2FO7ircAz8VrUXAA8%2FCgt4TckUWdJ8wdl3KV18bc4EbZw483uykzGbZz6q%2FJ7UtIfEVTdQYellcoDRMmcFw%2FYjwgMwFqNKg3cYy8FjovExWtfNhn1CvA4ZNJYy9S5etOXmJ8BwJ5gsOs1sjQJGBlqxVFjMnExqIXR298E8K2IJKRByT65BE%2FzMvKvawKdFAJi%2BTV%2BwEk8JSzxQ%2Ff40%2FLwJK2tmAhZL43QMkL4XqSoWLa1su8oZ7R7Nd2MFpOMyTFI%2BWHxsYolbuBPPKWwAuZVAb6IOioIH6aDE8%2B%2FBOjUlkLpZA3jgYVkqVW5znwGc5fE3OqH3r2cfHAX9EA8JbcLdihIJQJsfxV1KJIkLaMeDUqNBxFIvMVovbBwW5JSJnd79n8cb95RkjM7Grgl1da8I6RNSjxIK2ziXbmR2WxKoaZ3QV%2BWjk4TvK9dkSsie8qGY44Q7yeqd6Mug%2FFVc6UHZ%2F5qx4v%2FuwWuRbP%2F7lCCfUG06YxN0AZv9REAO3ghQ2brV3iuLOQNoEAIIzPix2K5Anb0fCXgIulHHE38lWCAp2otK%2BUh7vCHIKyQshh5nZRBUO7PFdLhWvkD3lHCFv0ILjTBa2K47D1mXylRwovHkebtZ9gbvSN4huUMPT3lsGh2SGMO7owMYGOqUB57ZkNgckkbJNFoksFaD1sSKfv7jW%2B3XLoDbCLIwhd6flGfxHFyLHI69kRLFf%2BkDG%2B9KXeC5fkufGP%2Flr9gHyVnhrVEYO4%2F21yBMH9VV4zFkTPx7e6V2SP%2FOwsBEPsnNLUNpd2iYvWJgkI49%2F3o9daxUYul04%2BKfdg58SZNF8CO%2FYYj6tFGHHZ0h9IOhkSHWwdEMLvjOqpj00INJHtI9DxCzYATyt&X-Amz-Signature=4f345bcf29a6162f1a6b770bfb0113cb26af9d889104fc896871df124f7f301a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

