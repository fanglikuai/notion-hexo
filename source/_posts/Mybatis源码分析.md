---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665D6B224M%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T020101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDy9m7wN2p9SQF%2FfB6rXOFkPgr4YeCqs8IYIm93MoYA0AiBgkGecCMFDJnCM7WP8RmVpV451PzmYbdeLDdeyouvFtCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMauzdm9tMM0Nr1pxVKtwD1QPzpt8MBQb9jTFQb9cPTosWNjUbN3KWNdP9mBKYuLRRvpkj3ksy6gXMvFrz1dKe9QE6wfiTIZRx2AVAhnqIekNqO42%2BegClQqh2l0fozkquJob0VXtj9gLfUypUeBI0UFSNdpPZwmEDSW9Yn6qaU0vBuhAGKxfMGSgWTHanxP0s1gSULTZ%2BK3RzjljfDoLqIFPNUpvLjPgr5l04wxngEMeQwPX47bjZXXc5m0jM4LaVvUw1PCIuwpeVLy%2B5WV1em%2Fji1tLttRzSyJukpDAG8JSTWOsJ07IzifjscUCCKCELVRrY5dFStWjywzmn6ZNGHU%2FojHctdrlFO%2FO5tBgmsBg1skN12msG%2BcMBUmyfKq%2BrQNc03%2Fa2XyBgqs41VuX9imRkadVNDhofKZy8uIaIotpa%2Bzb5qSyB0u1nJeZs%2F8f%2FmYTkHuzG6%2Fi%2BHgKYvTOXuqm2UrxA0QdKodDb94pU%2BEGcyyRCC6S5wvGlhRy3FM4%2BD7aLgHEY1OwwsdykyD6IQumWJxS0XJVE1gi2rH8Bnibw0I2%2B55bN4%2BR2pU4k%2B9YmUt855ljddbkDbpphbMa6D71hlTDEPlS1Ixn9nsyjZYo%2BYxCf9FvO3EeFKBsgIZOrC0fgFqSeE%2F3gajAwzJSZyQY6pgHKDLDEpRuM%2FyyxDUmpmewgfAy7nnzhQpAPIdVCCofVvBSpNDAt0%2BaZSia6MDNlg7o62ibF8IxRwI37wwcEXeRgyc%2FQNWv0XLWPMUcLTGg79YLN5wEOpg8iQZaDHnIsLhpZpawwMvdeChObKMpNMOvfCeyV1TrARNwa4uweWfWXYZM1OyZDhCa%2BeudZOoBw1eH7o%2B8L5FImvBIjcQ5X2EROjHJGdDvn&X-Amz-Signature=7312122edb220a376507b96bc670e24aba031812371c1d7d197424c64761514a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

