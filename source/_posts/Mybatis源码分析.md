---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VK472J2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDnVAxjzQtxe%2Fj%2FJZtFD7U1ncu3ppI3yDvh2p%2FfdTbqkwIhAJ%2F8ZH6eBtYQnyPQJxihUdwB3jhQ2Ikyg99pr3LwrCrIKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwAIatjgLrr%2FmBh0UIq3APlk16DeLqvRUkux63l3EujFbhO4ZsSr5KPWFy6BSB2z0L41H721h2cyMZfJw66glHz2tq0aKHKpS2Q7DKEbLkjrbenKpVtPGzhd5TTVu3CX5T6B6ne7xn4XnmUtRPiI859nd6hXIz7mF%2BdFNE2MZWzn8mAR32ywerhufCkHAmiYVwjSGoXbLsflogKEDaXSPidMIl2bMFKzhHj9hf14KxwM1rLC3QeAXNCMPY2rOPBYeNPxfK2pTKGMoX6x1cPH3l2gLfxi8bP4vn%2FXuQBtrOJzSywuN%2FGRUm13Vfz16Xlrb0YSnbB3F4GfuBrAiNsw0r%2FuE6XYoSSnDu34isOyvbi5F%2FX%2Bh9vT8Oz2P9wFrmyNvfBf4YJFl5ydq6%2B3RP8YcHQdZm0i%2FaNDjEkJr6nzM1eDPrMiVrqi68ePUxatH1sOVKgyk3iACD3lNk1PzB2d120Infg29rtQ91y%2BwdmXaqKRAbd5iDNW%2F21BteabpuczMQTbgkpvBmQqI1c7RQJ27Jr8H%2B0u2wpq%2FcPTG0qZdlbGH3Ztr7QwzGcLHevxJz2gbwEGg56ASDiLvtT%2F47H78sJzN4mEJjlEaPeSeqV5HfOu%2BIBfwyZKM6hDaAm5lmksOzJAX5q90XSZph28TCGhfHIBjqkAWaH29H9pweqjH%2Fih0kynaUvcW%2Fl1vwEDuQqa5GI8IPK4eOPIlvkv1o6mkQbC8WO%2Fzv%2BtnVj3tGR7O0UHV4NS6oh31%2B%2BLYauEqjuiQ4Jzvx5QqH7XiRNyXOysBpuI98U5ZDKvuVEA1TV%2FqHeiLAiNewXiChpodCYGEO9lt2Ixif%2FXig%2Fu9URlBvv5jTL5%2FX9VuTmUgOOX1ORLXecQGMKPKtAUble&X-Amz-Signature=062794c302ed499d5814cf56ffbe640a132c6b6df79314ae9c161c5e314cb9bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

