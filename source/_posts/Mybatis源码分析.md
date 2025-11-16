---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI6LMTX2%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGcNfWSZHH5ihrC6PHAxUX2pgu75vlI0CwKe%2BbmImcIwIgMQaJlXt5EGf4Pdqheiv53Od02zJ%2BHjk1M4tbD9VD4ecqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK797oZhWBWCDoYXECrcAzxRXQDuo2Er6QNdQxMe8cA2tqIafZ4pqI6ox958dLYYmVKOJkYy3cGmfh0uXSBwqbErOqp%2BwKilwHg2tDvhjwsO7nmqx0HlQIV5yvp5PIJF93WEk8DGjpmODu9OZwZpaJEcBZySmEEZfC7Jgl6cVvLNEDD1ShLmvkSGPgR1vGggiExFJJ95RxLiwSN4jWED36JoY1xKWZt3coziwT9Wrl1EGAu9K2aTcH9Fag%2B1S9J916LVOSU2b28brYKyh2J7A%2B61VH5wSV4VVleDkN3A99rAbEeF4LuWEbX2IUM2CO47dlbw90W0LEB4O8Uew1yxIT16Y0Sf9FBhy4B%2FJFrs0%2BR46Wl7Y78xcEOi3fURO%2FKNrnbDACLbrSXHiHc646zFtyCj%2B1fvYUGoLyqENG%2FnXm%2F3r8eCuDpRLZRyAc01%2B5OkGcwqYR50%2BbYxDvlNNqm2zYXlikqGEoDFqn7dhn00tYUfLWI7MaFcBywjcDVfeXS6iMpgxWa8yngoj6AZ6KtW6NBQaQTzV9l%2BNHuWKiIOSse8%2F9QbPTMXcw9B6OY4%2BzOrK3wG3W4ck2UHOl5tdP24zADvxTBKckQgVq3Vi40%2FNZ2xW0Vf4%2FsTBTrFb2CC7dJjzY8dsrIRpnJdpBVzMIrP5MgGOqUBEsGGIfesnEHo7kZVEICL%2BmSNNF%2BKGe8x9NxlQ7DUCWT6RygNpESO4OXDyAWjXrc%2FFMaVSZkzPOCor5bfFXcAW2EZsxQBucrGW76aXcPfbl8ziGdnyo%2Fr4Nm9JNm1%2FagdeLmbub83847hJ15ospCCB4VpHUrlubIhBMBG6ijg7EdO6r5jyO6gWneMVOilu3IkxbLLTpPNNqba78qUgyjEYgr3TKme&X-Amz-Signature=f1348f9b6f888de9ab2b8f7ba8285e5e12a2a0fbfa683ce4168d26ad17648d1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

