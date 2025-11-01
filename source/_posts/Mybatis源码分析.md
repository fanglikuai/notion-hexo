---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROXWW4ZL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQCg54wyZkn62eOKr8LQr4LtQknFo28tBDLxKibj%2FQoY6wIgfKBWrqJCeCD6k3llKGjmPaG1PN%2Fl%2BpVYTG8T9b9z2Dkq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDBOyxsY9qUpTFFYSnircA%2Fq%2BkfnWhwJf%2BStu27XdbVyNznN2udm7hPFiMY6xUwmD1V5jy5zDExro86OQg9e%2BLRLNNm440OdIAuvbZmuIr5%2FY0bbMv1BUBPQJLjLeyrTcQWNFC5bKHYdPeAAnRM34V3TAJVXIL4k3YGq9%2FVW%2BW4oRnlk5nT%2F9tBxm46SCVST%2B86TDyV0p1rDxkMyRooC0%2F9Hct2AweheKMb7Bk%2FJ91T%2B3M54aP4yM2x%2F6LOsfTs%2FXhexDW0unYPblcNX3TUWcH4pR2fONiP7conMMAwLkp5DyscprIMuS2%2FQrpMeBZMv%2B8gZZv7k4nCafa2z6z8U4d2O4lzJeujCVn7VzktOo2LHrcNa0cxzHplWTZQu2F6Dh9FSXeKZ4TrV1ynUntFEEMBjYttx0rBb33o6zBN2xnVhHOTQw%2Fd9IBnOEqYVcNXyAgfCNwXJlALhJ0SKDnAAG6smQAiPSlYY5BOZuvhfnL5%2BHKbscTL0oLW3A50tuNlEH9PAZbdVOtaRqbJYKOd4q3lunHa7L20SBDc0LoHGVQE%2F8qoOOe899lRTC6xz64Aavn9rNzUYy%2Fr7Tg9JxmLBielizqWYqnGevG7X2DxROEXHMKUowsC8cnHpSn9YY%2FDdBhW4W7zjOEqHH%2F4fgMIDQlsgGOqUB6FM8b8BCPgLe%2BtnLPNTbiyNG3jk1esn9ms58ukP1LIrh%2BzDkdcPbSwaUlUPpvUcldDXlnlbWwE1KZiOMAEidGUxSFqJ89hPkNu3HtXb5oMIURocCGUzuSJEFHCS0BsiH2HQCVMEU8iHXXQprV0vCqfiYRDmcmH%2BpvKUfPgnhwl6obg8WRfIHREmVsjOvXIYrmM2twF4j5d6gF7bZd0MMMb7Lv2mL&X-Amz-Signature=5de669f5e68c1112b13b484ccb5e9bad6967932bd34f78de4e406c864df8005e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

