---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWIQ76JJ%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH8yrVEfbhSalEfDp8k8UCCwRUZtP1L64ftyKAYPGxkdAiAzWqa%2BWAg0ylCit8FNc7xykTqJ3ayipOmGKv07f2pwriqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP1XY%2Fum5S9fVQx6gKtwDj6PtNu3IjIkCnXV7mWs5ImbZkMAXVQt63kYBNkI2B1bRNW5gvT6RYvPL%2FAJRJms6WH0ESBfwdUga96Z2Bw08Tw97vjBcW8M7AbGP%2BnvDuJBZ6W2IKNFnK%2B5nzoBaaX1e9V%2B9qzF%2Br5oLqliEVgLw3EPMA4kJ2cdoMp1UUTEF%2FiZBwGn%2BJdT2mGX426xP6YLHOeAcrb0qI%2FB2BgcnE1zikAkrdLCkyPHM21mbRblWpQ4f6hZZIwLrUi9md8BruYfNbGcvie3EokM%2F06jPIkQxRT0Vl1NVMkmn36mRqrxRxnBSdfkJeMnuyEXn3NyHNeFv1RS961zAPmcVQKy0VzQWgF%2F6KEMiFrDYdEVo7s3fPY7aciIK0YOcBg%2BU%2Bg9cF55DEnH0DqwRgv6UZxgc2qsf01XTKY7wrrbz6U8I%2FMD49J%2Fib1WYMtyCNYSsR2aRNihm5Kk5cHllRPAwuEVBhP9eiW3mVOC4DJdwJ9nMTk%2FBagnC3WXg%2FHWwGxdpZIYCnasJoe%2FUitCBSmkkOFDF9Kn1ijrblVRlX%2FH1MXi81lMu6W8sFiDot%2BkJ9VkmG1CX93SXarRCTPBZs6EhuiqSg%2BHzLvvF8I5EHNSwLROYeBeeCs1oUlqU0Sf9ikKDoFgwspL9xwY6pgG%2BPQlJQ3erVcnaXlHYF7vazYczn%2BjLkqZ6Qwh3iHNPUbQeHVHS03eBXnGx6km92vdWsBJk4juc7S09zSHCvyUmGakKdknmRht1Q4WQQjYp2AY311W9vS88RaJA%2BriMOEjR5LuwqpLnP66g0Nilue35ZQ8cSjm%2F7W7oC2ZC%2BW%2BrTZmS5nPpexpniRSdoByJ9LJjKd62wHTAzHpANKLgU7kddyNHitOC&X-Amz-Signature=451e2bbdffaec901c7fd52bd246ff116ea58a1fbec38c76e72cd1b608e5e7400&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

