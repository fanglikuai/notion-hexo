---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FUTKEKB%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCiDmAmcmZEpd8lClem5QUO9gGQRwqMZiaWl0vT8DzPtwIgCHQfM1msf2dI3vngPHm8pAVH1JaMfI9SJxMQJyD1FD4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBttHyQbQ4VnsETWjyrcA6Jy33L6y7YNhz%2FquOvuxMSakykwX%2FbGB1pwwq6w8sWgh2uvJoJ2jMcFFx00BUjFyr8dh%2BsLBj8695ZE2EXz1l8mQftO6sbnmdyrfeN0jFcyHOqzWwb9BLxXxVvuX1Y6DbFwyruHuSh465%2BStHd452ooDavCPrmub6SrWhRZQ8Vb6%2Fz66vJMIWyMkiKntp1wSCg4MY4XvWxB0Fb01CJBbQEJNHxUkLHWDyMEnFspPTpbx8keY10CeP6tp8A09dZHYH4TdUoJCRK5tTkgE9jTVVnh%2Bughb4MXupQNbIoKkuSVrbXjA45kTgpX2bbeglZ1WLYxD6WnZ7KikUHujmjXDDN3rtE0aNyet%2FaDCST9xsRUGwN0yHyxphJz2SGCIaRz1jXM7UTjIZ%2BQEkY1A%2F6dzPfNC10SsJgWJfKGRBgzFxP%2Fq%2Fr3dXXLt9pGaXCg6izso0XZqsSRt8Z4xXHZdtpx%2BWw7%2Ff6YttYyocG4eGS8iygHDDrenxfx7VYub83bt4T5PHLmDg2s6TqqwzUWMPM4Bg7E82uUFbwQtOCDcAbh%2FJCNnx%2BduYxFYBvCFbjqPwFoLLO2r260Ijl7UWzVNDpvhdqxxdC5O6vgbZTRSnd8wNEYkx53HLcOTz04COQDMKKDo8kGOqUBTofNeuCZW1cdWvgZS0UL7VAnPszp6dg%2BBuMa1E1IP3lDQ3%2F8WL7jLyjKdkTM1yWWeIe2nld3CtDXJtWitffuy3UW7DAdkrw0yOWKzJ4oSTj4Ot0Rhe121QllnfV1AvQ8sSpTf1TlVlmh5iCvQT%2FdvOF94zr%2By89RnqkTeqqx7yoOQE5y5nT3F5Wj%2F74N0402hPbumO2Z2spIw3RoEPskCa0RNNN1&X-Amz-Signature=5ab577de34f4a220eade094390131cd75f3f987aa7f520d6de494be1beba3c65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

