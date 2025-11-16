---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIBUMW7O%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdteF9vasAY%2BO4KDloBN2tYTk2SQsNTgqQaaIamz0LmwIgaVu1XXorcynFVgC7ubrhCFW0gYEw5rRdwsVmi0zQymoqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH9AYBnOU2y8GuONISrcA%2BIauJzEIOblYSZrms13ao0JdwVX3HM9%2FvcpG53JGO1BhNZ5zzT3NYSnk1SSZzMi5PorHMDy%2BD%2FWMbKIZMBiKx5OWu2Ki87PKt78pbABJWysODQ5ett2xF5WpLkNFifPyiCbgZZlJErcre9W7BwAonBIT78qnSYjp5CkOLm0KFhFkMc6g9DEA%2B%2BOg9Jaa%2FRsHKqVYlMiI3czgTHl2hmRtFauK9msKcwazoEnLy2DWeXo6c5oB2ZQc6YF4hKBlCeiLPte4jQAguDyQAHjC1H5UfdJCSyMCxVssefiUWIWpBc9B%2BaUg5URHnv4vFEzAHFs5DoLhSTuyMQ1IU8JSosE7Urn8c%2B%2F55GngpzG0NZpLAN0jzU%2FyBKvV6aVaDuPkB4u03GPPZi%2F71dJ%2FgD9T5Rh%2FBzHDPs782dFOFuXlBKWOH%2FGJyg18iCle3Px75fOd8WJaPeoUjp9vMExuuZrQLLLsiBmU%2BlgZ3R38nCI%2B%2FDda2TaCJPrXjHIfIfQss1ZSIzWomfaSsC1N4bYQAQOBSE7FnqtkcMffpAOPf3K%2FUCMAnm9f4NESFvP4WOHfHV14NybaPWpV13ZnjUxYgajQWzn3%2B3x%2Fr5%2BnpoHRQ6MPqyAHKW5iswNmkAdBxjA2bhxMKKa58gGOqUBK4exemkSaHWMfYyr1nPtQ24RgnnWWA5OfYe6EtQrbI%2B9aEoyQnRyM%2BLTxHz%2BImeos4zToJ2S0JKzuE7TtkMruhPddqr0TxWOUgdau5igTCFXN6KxkDeflPEyFmb8y9jfxB3xqT%2BNv8niCRqVjyxIVDsSrR3b8MF5%2B0QajFEtXJaDuuonLFBz95KgEG%2BtQEPjWGca1XsvqyBaHPuzX4a9jpuqWiN9&X-Amz-Signature=0561c124627304e05d4a9233b08d3b1ed8f01b27f4b296fbbda14ad80bf1942d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

