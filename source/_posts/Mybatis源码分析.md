---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBBRLUA%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJGMEQCIEjBHL2LsBznpLS0Es%2BmWbCDUIv2bRwgqFzCeNzPFe7GAiAxFGA3SSY331n04TSNE6A2gKHdM5HVIpwS66QnH8mFTyr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMBR%2BfTjmTGWWJRCToKtwDiXvJImEHX5PMN7xeaq9r9qCadzVIPEm7IICk1z9C4%2FBotKFoNo3ss7Q1%2F%2FoU34PTRVCvKyPsI%2FN3Go6kFtsAmYac0D5xut%2BrmRwm5oTn%2FhVDkP6ojjSKU0IEZn2upIpL9fbbIThiND%2FwCma5xAdMAgljcBdyZi6fC%2Fu6DCq4xpy6LiACEFHGVfrEU6Tql3QDd7Q%2FymWS5P%2FfoLPZNKvmjxmPVpeLdNnaXBmD3MTaDMR2FafN6xe1l5NvTOy4DKCdcVaOj9J1paPKtNRHD6E1acMyfZNmyVLRKeV%2BWONmbTSA6w9max8eobZw0w3OguwKYlZtJ0iQwlIbSKgMlxhbvu71ntfgppzrzZGDhXhjUZZp%2BRdKEzpL0NVERQAV0KFmNqFW6kZjqomAycRx7kg7f9cnKv%2FXMyMb5psecYnVOuZh52ATtWVcHmR278XmkIZzxmJ%2F1SJzPs92TU25EQk2jz4RDweIZNe%2Bs%2Bd96%2Fr8OCS7NFMtbNv49ILmw9EW5Ys6DklipTHMMSbdSZqhgIo%2BazrG1NohPsZwzo3v2FwwozE40kswNYjmdBdmMWM0CYfW3S12quIfqMOXUaC4Y5pgvT8VlxujNi3ib156fX1xBaRZ4pd9CL7zN%2BkthZgwq6vPyAY6pgEF1YbEsSnd99oxVmZ6mJI01EydDnX4TYToG4ZsGmvbrRzilzr%2Floiftdm1tPx6JCoOtBCJN0l7Qpb7HvmfdSdrrQWCfn%2BeXS2jxnEawe4C%2B6VcQqjs5dXdZxA3%2FuZZeEt1mL4dtg5UscqLEOEItVpM1p2osaTvlUvKRh7GSOUZAptvpeIkYnuoYAnJ7vsMqm0y7dRqljX2vk7%2FSWy%2FYm%2B9TdXxPnrU&X-Amz-Signature=82ac3d5204c281855858961122f72a3ebbd1f89fdfa435e4d7cca635c75fe9eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

