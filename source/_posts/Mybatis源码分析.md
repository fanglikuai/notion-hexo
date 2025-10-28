---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOA6Q25O%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEOVj1X%2FlsOeDS%2FklBd9JPDmjF%2Fuuw%2BDOhUAbEqVUv5pAiEAjI4ILFoPd9r8eO2uLbB4JBNDVfkEnpjQCuEaH4MEv1gqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFopVwTYsjYv5nU78CrcA4zytWFEs%2FNM8F4hkx%2BnnH6QqzOes8dZRwJRU7WQuQZWDK1CPCD4avHOPbMOJ3zlunjOoicav8UXCjguPs89TBEbt0wVuxgOmYygCit9U9rwrLEM7kB1Fnd90C%2BPyd%2Bsmum2HUJ%2BDErb4kiqvtolRNjfMKkWPd2LwdYOrc4Zu9AyrOs8MU5GNxJl4EyhNs8APOgGflGR6HcUEauUqdEFv%2FAJZiFL6fFSUEzIuQVhpdtlChgSB3W3RkxSdS9aLOPRG3ZXteqX2%2BKXg4JUqEtolNOXMgEnVEzcLn1k%2F9AG46NtN2LcYy6ics8GVTyy%2Fv74LDLbHfCaHt7nMh%2FPJQ%2FgeQp9PL4LPi%2FjI6pE2nwVfUxTC6A5RkXI0asLkyvf%2FlNH4bRscNZSIAtuY236xuWHcOQCVuoatktOKfZVpBeX5xx2lSD%2FwIPokOo189ONEsVywG81HQMMZxZYz1toORyyAiG5HU5WzzLdYOX16hPF826zH6YLDsvsPFvs%2BY9h3hajmGiaytF32kKutLHD55EN2BAMOdRD1VHwQ6HU2aSQJ74Cati%2FfNjn6vF6eLKap%2FweohoGVE22vYxlfRs5RcWzQ9%2FuweSPgirfcMUvMsFqmw3GUZwI6psbNMCYwE0wMJC9gMgGOqUBiZYFhZ3wGXiCkhRnZzE1GVzESguhcPOVDZB47AW37wZ%2BwvMpxuAM5DD3DDDhEUGKrwnF%2FCxB5q3w5cQPpPl4I4p2vCJ97dWixmBdNq6u3DzOy8h4ozBSfaUNWdeq0yCyN4WbB5FJdeEWb%2Bhsj7gfIZjQByfwRNEibFVs2Cl1PQFLJjg3csccTa16X7yQcKf%2F18fpqTZiRQ9c%2BTg6%2B2Lsk%2BC457Ko&X-Amz-Signature=08133cac32476a5864b5d3631e46276c9fa44445782a3517becc494298158754&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

