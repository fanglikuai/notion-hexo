---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46665J7UW7D%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIBQ9j5AlF0Dh2gRVaH5CpPTJZs%2FNZT%2FoLWt7sIPTmJWiAiEA2dtZSKr%2FHAsBC1wthKyTRbuCXCIw09vTAzjyIFx2MGcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKy3%2Bi4OrL%2FTxcPdNyrcA5%2F5vo%2BHjW3fJz8h%2BzXDL2fNfGK3pcCUTRAkYcgf1Mk73Z%2FS96s7mxoj26bPQ501V94YN5Z7fY0g%2Fc%2B8njLy35Ckz9SigpYRg%2FTMuF%2F3b8ZPc6XRgDr77J9uYnpVEUZ%2FhEYsyj4gKGwt1iE7lm4umkDpywrMSjxvU9USph4EqolWeresDCSokRmJlwmV7CYzO50PIQrs%2Fd78%2B0%2FlK7hWmg2uuaFvAKKu4RxLtuZVfzA9HiWVPm0Wqz894nZmoRjCdzzWP4bmhMHCzPTZcYmsctIVJrEAqcFTAOd8dYHOLXjOaM684CKBR0Z4uNrybsj%2Bm1X7%2F4QMP2kmpIHviZ%2BVjOl5AGsICQcAEIxBkrwM5rV5iWg1mWCWa12MpLmxNGsTgC3jDVMvMCMDNusRlk0k0KelnvdB9BD9g3%2FIwML%2F0KntIVEj%2FElJyl0TSt0d7V9D6k4OQbeifZeapi18V3JKVqZMvnadRKfX21Zt124dgwhOh2re0smcIAyzLcqGtt2KZa7%2FjVvEB2nTgPuV%2FA%2FwIDYINNmRlDUiV9DR6UJGjaS7jdJFgOPp2ZludjOtD41xFSFwbq0P%2F08bAYgf9TAqlxdUsf7aNKdLo%2BEi0WblDASS2aPs4TXZYtIrbtI7MJmHt8YGOqUBTOt2lEx1HAlJ%2F7zKYfUKWFqC6wgbELY9KTpyAMi74ehWZbu2sZkzFjS2gRuCk5lvxh53xc0FdqSiwqDEsSXz9rM7BgMqKNbaRiezkH3s%2F6yj4DuhgXNxadUmfrH90z1t5L7dGyI%2B850ErSuizp%2FYORY0lEA6RXTopYY5oCUXvBPEeNQOuq9A%2FLaS2qo3FFJHsPHFiPrH3qe2n8Z0bNK3o2H4XDGj&X-Amz-Signature=13218bfd9c9c99a2c7167147e807abfb172817611d5b5fa30a48d0b9f541d1a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

