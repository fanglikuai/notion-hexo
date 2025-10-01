---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKJGPLYW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T190120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgLgICgXbLXdNOfN8x2LkPsHNwVnvhUVKzpWGOOciNkAIgdcU0rVrP2ARnDiIqBXy%2Bi5vioWdEnYUuEToghAIcO5cq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDCINwoOZ%2BTpKl8xovircAwg5La4Vh7igvfF2sohx8Cu4xeMmLPnAzOEww6S%2Bwe4ACM7KuLITi8tDoUzBQj6A%2BUTJOYcp9jme9mf7xHCuEIATlc%2FuCPnM4TGMTidJp%2FCsFfR4PGZSrSYY6L2n3RurICXIcVwtC3UtzEucGhQLsAQOtlHXmZZMfOJBhG0rqt25wa%2BDlp4teIvJDi9CjCCYwN0VaI3V0wIsFDFh8WmQ2RcTcByYBI4IN0OoT9H2%2Fv1pDoBlps9YfJilX5gAYoaAuHRQOHPBPMOGlPCm0aHMhvHfMpzwS2nHSVSa6bBKIOQBu5FRhO%2BKGAHKLDk8KxiZGK%2B1fNEWsypzUqGei%2FhE6PeqqOqVt7eJnXjgAOS5oUDttJnzMXPsymCP%2F%2B7zJ85dp%2FPcebQlku5jjIqgiMGcBDC3LQQ87BrMUgUAVkP6uUmWfiT6PDWfU5QQGAGbd1tMro%2FVanSR3sdVv13j2eNYPF4YTBBXufzMHqyWxEIGduZEjUT2uVQ0pNtIVccmKjVJXpIL2fRE8xuXEzMrhN%2FDS2KoEnuROXGKfDQzOACCTrr3PPTDNfyhkonYNmUq%2F58hTKIIhG4%2FpVX8b9jUGBGHhErkMAuiUPmx4Xq6ZJ%2Bx1ie2kCzlONB9PMiujEvcMJvz9cYGOqUBwE7XcdF%2BM4XPrLgmwlazcOx11CbOxV%2Fh%2Fd%2BbT9xCB0lW4htNyteKQXv520E0X5eIY%2FXkRHOHgfl%2Fg6w3Nep%2BAGoIA%2BG2xmminhp4aJr%2Bm1rMMVR5YmWBTMiLrTARwTJouFf7ed9VLPhf0b5fEMKiIE9PILdqN8dA3r6JhWYwNHdI41GBn1Y9zpBiHUHiY6ggGG2u3P8kc2OWoJLjjXemncfq84mI&X-Amz-Signature=32a6d9edeb9c0239b4fb2f670272fc3356105ab054fd0b94c94106b0b6426d2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

