---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VWDH2FV%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQCbBQT1lPyRxen6eRKcvI%2BXSUXKeX7ORXDubz8NJIW%2BFQIhAMBwJPUygfGzSxBVaqnAK4Xu%2FNWeKJ%2BeHh3ro24bBseRKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIE0KyizJcLAOZYKcq3APzd%2BoyDQCeIAcXyCTKE1AcdzYhy2CQN2iFW4qwCPhZpNrmlFp6Ry931T5RGpGyNm5uNL%2BvR9MtjxtzadScBmS1sszJj0J3CJgRu9kvkNsC5l%2FRF%2F5L1nAr733ExaydgVWkZzIrh1I1lKAC4Qu3ThbPB5BZF5O7M3xH3E%2BwVX%2F1yT0b6hvDdRNEgernmz81suVXrLY21AYDQD7dFADgvz92wqskVLjokLIjJyo5u5ELX5guotP2Ih7YBIAR2LuATCOdJdWF022hHqLE%2FWXggHoOMt9iShXPrFQEtA9DaYL3%2FaqUDg0c1R6TTnWz1SQQY9KG5w93KRfS9cdtoTw9wOZryLBKa5LYAFxjrPzI7UZzLLhjkdeJil6WJ9sDPW8vgCyf4Q6uQOroX1EHfS6SzAhZ7OtcHLsMhhItGeS3eTpnZRxMYeuz5tceUQXggCC4SIgNrvDwprrd2I6u26sNOeI9JCkNpt0ZEE6CsMurh2CKbA7aCdggLOy5dv71NJP9lJadx6nllcvHpvzNzDVd0LBU0MoqzVM2gmvLdKnIozwfxx%2BREjkG%2FmDo1iC7xfXSjH3%2FNp11KmA1pfkyvl8FZ8FoKA5tr7Ey1WQ3SEHS3prdjXROmSCE5op%2BkP2q7DDrpMTIBjqkARqJWT2m7nsnDex1PhS9ZC8177sd%2BVb3Bp3owaMLfsQRlXm1kkKwpcgU6DDGFggOmCPn%2F8nxnD2iMUkBNcczcxabCVQJGUhRb70HaK54L8G45DhsnHrmi8zRZuxHWWWypTM20ACz5UMHLK5fzWr9fxmpw11kdqSUcDxS7hNfnDw68ncNIcd%2BnzdCT%2Fnud8kELZQSQBZkhBfaCqIk6qJb4bzcPl3P&X-Amz-Signature=2fb04a837bd1b4ce97c58b1486866c1222fb459815062bc4ef14a136ee4d2e91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

