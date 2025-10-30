---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4SUIJ62%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIFWmXh7DVIxRnqIFq70bBFKsuHGlbO2DvVR8B84zBq3fAiEA9YC4hBkaAg3AmjvMb8aoaT0agmWDeAX7rTjdHGyKcogqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI1LCHCGVY2Kk6Ts1yrcA9WaPn0F75KqKalkn8SoEWQRgveWH1GN%2Fg3CZNeyArNinzxqiHXaXY416zc8g7yVpXXYAOT9MoC2bDTk5oAz%2FKmdY8cuNAOtG0mLN858uGs5GgIzY94YAg1dSsyefaTdI58ksov8MI1P1FEqNFeb9PMdvrJde5Zn8rZWDlpE6Q%2FAz%2BvK27NGEJwbUSYZd8ZvIxLJuuOaDHnG%2Bidx3ZMk5bS5UpkEKKYH1jcZ%2BlBf1XaCT1OaiCTvMj26wrMk51%2FvwPgcwEzmPCqQ0LR6tmJ%2FojFNxuDEtOePLLhQm7QZn5pF09K2FYbyUP4WlOiWecnGMGb6%2FGDsmLWNl9SgnY%2BdPYhnYUtUTGf8C%2BXrHpG77On969UIB%2ByB%2FVRjRMQzSU9ysLv15DMbOhaeKZsqsZ6aSS39MFcmJZYPYZJV%2Ffqhm56%2FakE%2Bn%2FPmN8eDlQ%2BsjXvE3CFbtvF0eaTPgYf2aBhg%2FHtYcURXcL4krunqSC9PP7FostrNTh5gn0Px%2FAN8Fi8I0M%2Fdl2pjA%2FqaDEu7n8SNsGsbcaDHyfPvck5UlAhbeMBS2xqjAAiyfcUOHKKIRZbYzakBR59xBEJefud9kC9gzm4%2FY%2FBFOUS882%2FB6IQ82mBByovTXvMQUOSmFlZ6MIy8isgGOqUBvFxJSIoYJWYas7Ewz653HCycOX%2F8uy0IjxFddrb0YLCY7lLYnX4vuNQd9pNbyuTUztME8Ur0bXUSniV9HxUpw%2FLISStuL7DNQ1cBsA8UUGnRzieBwWt1JuFnr35BqCueclVeZTm5tAHHtWVC8i1Gsd04J1W0AG%2FBWWkqbxxl0Wvb0VP0nCZescyiVJ97e5jUakj0W%2B6KftidwcWOkt2BgaP8kjaV&X-Amz-Signature=9f7d1dd0ff7375dd79dc7d5a9fa14b3d08e304d0cf3e5fe37b8b562f1ec19144&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

