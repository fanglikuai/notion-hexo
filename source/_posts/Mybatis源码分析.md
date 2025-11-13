---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DBIVIOE%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbmphMPNSBqYUlmC0HbrwLcQG3YfHjICm3WTWRErn5IAiEAlWgY71Ao7flrmiLslSdbVXeb9BobJ1kl5iQSdcM%2F4EUq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDM3Z9%2BSj4v2ajNFVyyrcAyx1CS0qR8h4v4GgDttPwx41PumZ7LmrCAL6LKAHmLyMRCIkhR0D4kTTUR0iXBYKTy5iBrLevWHXxYQ1oUVPpTIfNQ7SU3V%2BXDgNqRBKc4HOTQp8gSGLIL3OwvF%2FIeqiSxi9jlz54nOAE8NlAQTIq7mLaeAc1NZoJKX9ukFvNyft6t3amesF%2BIeCxag8L%2BHdDM2hyNvQd14rLTALxTbe75h1RwtT5R9%2F0Nmj9AWYjmO2gzCGgVcDx2LbOM8Yg4YuMiPoSiVKw%2FR%2BHDBQZGaBugh%2FELchyvANBnhNclrM%2FyX%2BRba3IHgiyIlhYunLCOgJGiKastV%2BknJaSZiqUp%2FBiQYMT0F%2Fltq7wXg92v8k3eVGDwzAkkBVa5yEAs%2FtFmgrQdUpj5Ye7ePtUADmqd8RDskBhR7VoURKOR%2B38aR%2FEywDShkogjTfXMaoSW8yfPkRrJmgd%2FObmQG83Z5AAWXbmfHPOtLQj%2BIwL0pxeAd%2B%2BOuehY6WT9f4ZD1UgEM%2BOGh6HXExaKp9%2BY1zkR%2BGVzCeQuwflBdVFh1ycofHmYar4B8iDBrxxv6VzxeWKVgDqDzItZk2TD1A%2B5%2B61GMDxn0ggD1MSDrbfy%2BUJ7BCEelIk2aBD5UP565IU2MVUUJSMLrf1sgGOqUB9nEg77SbeQg%2BhCX0amcHERsUyzkt5%2Fq40jnrwRjRbTmWEuMx%2Fsbvgv8xnqo3mBMqvNlc0kxa8CozAGs%2FkE8y0VDGbUUgxiJgzoXfuSaJNT0WQ1t%2B%2FFe597HhX9U6bxwU5m8rK%2BuZNMjnt6OJh%2FxzCtALNdlCnehigYWiAtw1W93BPqry9tHh%2FZHyEDk%2BcQAZ7m4KNlKcJXKHxQ7JF6VXFxEnHchG&X-Amz-Signature=66a28f34ae94c9cade301247e07806aeacbc4d1d6d8ebaf9cbeadb313aa30f06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

