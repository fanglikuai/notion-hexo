---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FHF3OM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB%2BkmZTw9CpgsymY6muL2CNG%2FfUyBDs0lPfTP5y66LFXAiBP3duOqFKMNW1JAOxixKB2ZPFir9OckU%2FA13WMRvA9DCr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIM83Sj4krXfQygpj6PKtwDWUS24SkKChtblgeYvSnyNMDOeEYvPvofWf8gIC0ifofaySQfBQHzaayNBYY7xxmRfth18i4rJNtOPzer%2Fe%2B8na726R8GUpN664SsDXkDzSibleu5qdFnV48Jf4kUrHL%2FnvaSDa7dyAxXguM5l5Izd2DxYcQci1ZfCKSrf2gvUsM3gGPcHpUmdvUcPsqNhO1jV0iXu1QR4evXN45n2AT5s4mpsVpAC4PQ7Ic%2B3J%2BMAXx0Mi2MES%2Bm1HXp6DemUj38bm3Vayf7tLUd90nhpwpMpAoFECzKvqcHmHw0jSeBLfcCVhFw5yDrJlivpFCy2O4lfnD72oSx%2BdsSIgJsB1RNd3gLFERgWdPedjd6FrFUz1aJGqje0c2%2FRt7ipcSiPCtSPlK2RkunmZS2JPJVOk118vN7GaxF8IN4Vf4DaAfON711ktUUHAOLyfgDqhbXi1G2RvEaXoxbjnTSe0FtLuBZHJJO6r02Ml6RHdCqmrd21gxHsIjCDOJ8AH1EIp1LBS%2BmeNwN%2BZzd99sP7aqQJrwR1WI9twZ8bijJfO0mRrHBPsL8dB9je46G%2BYub6Mwbhbu3Npd2wO7zs5AP%2Fd1MWC%2FvTBFAUJvvdB9ZCzww2A%2B8%2FhzxxKDrJXlnbOc5PsMw%2BuzIxgY6pgH8GLnj%2FhN9HAO2tskd2ueV8nn7KWeRln6RpDAxDYUQCAFX1srDc9BvLBpPtNsew6%2BHUfavBkfw9foxSYpTFFAFsyl5aZb219UwKVqnQ1vh4fW3rHM3pPIWzzLrhU%2Brjs9oACjDifoukPI4k4rm23rV7eiAHqouVvsDH3LDzXXoGH9Vmu7Ex7MAAnQIFkNcKMTdw2bSXNb7%2FRT31kk04IlpE1qPKKKN&X-Amz-Signature=49dcdffcb09ebddbf3c9fbcdc960ec330afc12323ad9258d9e09c47cfecf1f2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

