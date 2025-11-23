---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSAMFORD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCscccTv0PNHtRJsQ0fks88RVQ3hlw2olhrgbxKGWIHEAIhAPjntWvIumRf43dj2GdNFaBxpiJegaoynsgg%2FKaUj5pVKv8DCDEQABoMNjM3NDIzMTgzODA1IgzjKRU6K8b3zW4KdDoq3AO58K207u%2F%2FAfLLcciIt9cqVHsHoZb%2BD7xUbpsi1jd%2F4jgG9SJsPuFa9RzJnzV7xbwHFl9PyDKeKmdVtKDRlD7iB323x7hPiPsDW6Ou3nD0dFqaT%2Bdx%2Bxrch84j4cvNwrE%2BYGMMhWQNCxKrm3fiSYo5Mk6wR0oAGeI5%2FESBRsR5jDNGAuIU6TIfN2oAM5U2BudeoRshAzpTUdOf1GysPhl9BlFCZwQztkrqBUgEJRwN78a7Jr66VPfHa1qBlKYfzc%2F9ELueys%2F7kLUGlgRMI1Y7gJEubEuKl6FCasLJ0Wj8BV4iEjYKtB02cGHcoP5Kup3P0Vipho3dzQklWtL9aTR3NzQ8Sd8pRzx786EMQ29QdqRw7p2dDQpNM7NTizdxMMCSEJ8r7m%2B9cY0zY2FCnK36VRQg7RyRF4lz7cHkokstHqypwAEPUDsNqjeZbkIEKFZ5YJ3G%2Brgd65OFKeHss93OvzV6IZ3iOzoh%2F%2FVaDpUwHv9o4NJ%2F9Dlpnkz8kBlzU5%2BIJDN9PTD0nvLaCtY%2FfVQb%2B5A7HN%2FUEGONyI6dDoZCKBW4aHnspXoXEi1SxmaSWWjDrH9fyqZZ2RItdtMDPMGgKetOxx2tVTaZLoa%2BrFIvax5ru6NykJlqRwrf7zDHn4nJBjqkAah9NYcY%2FMhv4dhJ8T3F4%2Bi8l1ZWQe7%2BrHULgqrBBQyGJMgGZMTTth0MAhZVTOF3%2FbnpSWnMu%2BkMe0HO8ZB%2BxkXahkF6csElOOK5BcIrXKRzh5Y6IG6zfVOWt4%2FK6Bv%2FeVII3WNN6e7UCBwNfQY3fyzW3iYZroEKcL1TqpTVCYSr5Rawe0%2FR7uMkTI%2Fk4dBP3Jqua0E%2BDIJmyrDatF8Z2to0WFj%2B&X-Amz-Signature=fc5d47d1fe1d293863895678e2e709bc8322d743367d1dcf9e9ab42f183b4806&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

