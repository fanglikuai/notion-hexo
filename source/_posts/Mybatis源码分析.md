---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQVJVWKD%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T140045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8hjbvTCPTvUquXZvr3PbRShZP%2BUAmazQRH%2BJYrpyZ7wIgQp8Szyp1ciry4kdeksLk0Z9tu5DNWWmop7QU%2BI64zwwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDM5LvACgKcDEa5DefSrcAyx7GdFjoBeQAZIfnodh4dgPtezVF6NBiMeL5adzRSqeF%2FHnRzN%2FrPTnibixpRiFJzUym0BRsWcu4udKnjaQfdsn7mjEoV%2B0HmaD8JtnlV%2BR6dDf6l4jKzWCI60OWNcn5XSYirtu9I7M3cIzvNfTPZ5PStDPXHiBbeXZFbNNHqr2IkVbpEc%2FMCo44flNgUi1XpSDcmvDl16c3LXm%2BfNFGlPKdNk2zWBQH5nsxi4XQQFaA2seXBY9aUAJ6KZ0DOztRBz4E8VullMtqZJCQVuEilKdtk9tnwopJR7oWtMqn9F25tMELFaoiKf3qkhDCMCWdvkckXzB%2BrBtRjUbE1cb%2FobwweVql8VS6KP1EHahKOmLa%2BTKnJdyIbtbDEsROA9Jtb7WGl4djHanFJK3J%2BY1gSn%2BzCKn%2FzaGmruY3ecpV6%2BKiT9KrkX3RN2WkN3pfuJ8CwAH3hQ8pBOrPY4i8HmR2%2Baocqa7SQFguW%2BpRiwtfcDDeApF55%2BCavkORw5d9Xjlm8v0YeyNqTvmx5lPK9pTK0e8fz6ucHh0OnuyXsEjyuazu0c794Eos04%2F7w5u2J8mAXEWAgzCkAjh7YkM4%2BKFNCFMFeWNBo7NsGTzfc50FUUxPMafftOsL1KhOFeNMJaD4cgGOqUBDC6Zr%2BGJikoHCy9MOREx6gp0r8NL9IaHAyRv1PBJNA4NCszZSolH2ddttqsepQQNR9kQNxic6eTWFkSRDqJpyQrOk%2F9gLbGnd7uGk4Tm64pZhSsSES0wIwU7JdlLDSEEu5632vnKBCwN4PNsWaXoQfmJdbL6K32h%2FwKw8xO7KGli8tou4LfLyfkErnhn3gCdC1A5F2HYNN6eyZ%2F18ffsrlaSlOkM&X-Amz-Signature=6abde584f0dae2308f9d0ae32838e8365af290caaaa4694a5c511b13b868ea23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

