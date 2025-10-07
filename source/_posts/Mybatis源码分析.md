---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7B7ILP2%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T150105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIB68Z9jlxH%2BnCbCoaEtBL2GctrKiDfumKUKh0%2Fqvd8PpAiEAvQi5vsCMM7H%2BotEYXHx8VNn67KfNBrynaeHIQR%2BbiI4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDn0EaKNOvkLOP7O4yrcA1rvA8OPzXm6ch3jWmgsNtAqzOsSfLFDl%2BmVIV4uIjRa1sEHszhi5Yskbho9XxxGpsI%2BCIFl%2FRcKnRC5wn2nX34O6I9OZeVfIdsGtyKbTa7t7gLvJLi5C0TYRUlfBrBJYOa6L6xaidXn3m1O0owBQBrv8FksAceuT2oJCCAmJWLxpzz8upUXdJQ%2B1qH4ZZqZhIFy9Pr%2BOop4T8C9rpihcyAbG2sPvCsNUn4tb0PusmpW3SEU65kBsjTvSkccauw2PsNHUXt4MZkSxyVMulHdeOOuqCPiTBStPKjoJrhVfhe2fcYzDKKuL4z%2BLK%2Bl1qWNDk7u2pTHHtWucW6U%2FXGVVuUdCp9lDKuHnaIUBtAaE72dfs2y1n%2BFPWiCB4cgFU96gcrG%2BJhFxPlf%2B4zPSJpDa23BZ0eyqvuqQ%2FdBfu8ZfFP%2FM2I5cDwtU5boZzSLJpivEvpi7Kysp6KTHFmimPHhpUsXwUya%2Fm6bsMN4BB2pVJI5CZBNyDZCoVyl5%2B5DWaIXp%2FkrFNmowPd4OqGVI5HpeBxImAGdxb8nrnJo2%2FeF046uJLYE8kp6S%2BbDuVXlksUQuySZ7bkBViwWKZSH1YnUPoYR4SPNvUEd8x%2FcWPHddKTKFE90HdnHMkCYlCjxMOe%2FlMcGOqUBbEmfdkD8pa1fJLBWq5ZLnojjGs%2Bd6OknhyiDYyZrJ2eRTVNJ9mIWidgyrES2RvBtoKXnk4MqDKadQsjXh2%2Fj83tKp6DG3jqrsWyEDMyXhQFpKX9sJan6z8tTWWPU0tU7mibcOsg95lsMpnYhQyWM11u05A23VOD71ZAkX0Xp9UITPp2i4e6oTATTrynY%2BvOhOOYbrCTrs8LlxenONg%2FZvxr08L5y&X-Amz-Signature=a5535365bea187df78a418b47f931cf39bf463dd57c1d2778a73fb9420d388fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

