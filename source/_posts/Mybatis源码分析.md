---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZ6GNMHS%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcwlOAcG%2B3tKoYpfC24m5ghdAqzRIh%2BDNQA%2Bt4%2Bf65uAiEAiUZtRcqzzJzLHo7sq2JSj9cB4XruLfjt%2FUa3eKSuxScqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHXIwuJ1X9mx2b1OsCrcA08CYn3QmkvwcxVGG8aTKpHA2dvPW35anIeNvMPH%2FIEc5aNTMhxpeH51ziGn8Eo%2FM9q2JlttbRyD%2BxwxA%2BsPi8J6ulS1M9F7FnXKcRh%2Fa7P%2FndkrUBwRbx7eT2iNhnJ4TTXNMty50ha8GO3GyIA916QqBJiUnBwXlFaxqV%2BUuQ4FBjk5UJ2nwdFXhC%2BbXbC4l3YV0UkUTmdKvgKs3km5b0fox88gnpSFTD%2FgP0QkQUYJMygycCgaOHZbpGEnsPI6GqTcqZXAfC9xn1kZwepGrfJOaz9TxCEazGx%2B3IeOj9Yt%2B%2BG8ECThaw74XcjDOD1WH5dzBUB67P5WULX%2BrVGjpXxV7TlSZpYjU4UyoPiIpJ2%2Fcrz17s4h3DjwXNFdFOu1hNMTHOHxik0mzQX%2BdDaDLdQB2c4Snmg1Q4EO%2B8XL8n0rnZLTOWQV4vgwlYEzrHw8JTz2GWDPPF%2BalFTQzt2ppMCydT3ZbmK9P0sqbJnOrY%2FO3dWCuDxp0vPk7fHUrksQs89ZhlQk5em8cu%2FGHnRFmpiR3SZWx2MdNeNHfnFdhxrERtYvOSphA8XWi2gGz9vHDHT6hxDcNLh2iJPVP1X7QZKIQaMMit%2FJbk1CSJLn2XI8P%2BYSaxzUGZZiylwlMNO9pMkGOqUBAbH6NfrxE47yMlGT2JOL1wrcNX%2FytoOz%2FXYHV9asol5OA6TqtAU7yvuyx%2B2lPBkHUb4H%2BWXEGV4c1lCJEKHK0Es7AYyS%2Bn2AVqmIzJ%2Bv2qzIU8TgOVUxeLLm0JNeWwSm59C%2FAc4W2mMCjCDDlsggtcMcwql8Q9uZNHPSNm11ea2gKgF8xi6FH%2FEc1uq4uTJdef0YpRSC5k%2BPmCo3ATPxdHFCjC8%2B&X-Amz-Signature=01734c1353843f1a8d0b43e7aee08df70c9c1ad11b4d47bfb59f94d8444ed7dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

