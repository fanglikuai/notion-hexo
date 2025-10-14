---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632HGTDNZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEa8h42jBlglnvtPz6VfkFSI%2FQ2rOFwLPn0Pd3YJMeF2AiAkBM3kCGsSthckv7kaHL2A9gq5kZ%2BQFfQL7W2tJ7cvLyr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIM3TOJrFMTV48ODYGCKtwDqelqNSP07%2BmAT0GCFrpPbJwTIfhoaqu8NA%2BVsVDyVgbIcCbMPFpbv3LRXSX2NII6cZmYeNSIFW8x6DvK6XWKMnS1zZYUGjDy4wUF1%2Bh%2BC30q2wPgLBk7PxOCm9t%2Fo2I6i6cX%2BEWIpa7JDCG96nAIb%2Br3vYHH9YSB9xPeh%2FglflDNFl9redkm5Znj86DLRBivrVwfcQ5GuMWdfuFRWZW3kS41JPP11yD4dtxCYXVglBbSbUXz2E0QO584nQnqNVS5N%2BX3O6C8tVMaYxCGGdb4YH02a9u7Urrze%2Faaf6wP01vN88jpaBwrmj%2FzUIDK%2BgqBxmfvrtH2R7cMt9%2BEnOj1tqF5YRe2673Cnnhu0PM53i73tAYnOUn2CGBryhO3dR2Cq%2BJW66Os5MC8Q6H%2BFARxCxl4j3VBlyX8WBdtd5Hih6JKle1oFga4DX5TyvQvJWQhKSRaooADF7tNQN6ycLxCbxsYT%2F%2FJdBXSVw7SvTmj94qnKwakjYGto1EFJvwoLooqLdWajdXmJ5V4GdXM3kEWs1JzSbtf12pEwTLSWzDsf76ViNpZDqTqngKqTM21UTWZCB9FUuoGSYyNrFRbd21WwS7g4eGNaba4rVI3S7E1EM1JLUXdQu%2BHh%2FOD2Pgw%2FsK6xwY6pgEDgFe177UUxuM6Ap5GhHnxPmyM5YyS0V41HHwlsIn4Z%2BhGMbpowhz1iHwy3xyy86AFXtFmuZ8OEW0FoQsrI7gT1uOfnL7jHfn21k6sK3Jb53kDVqOu6v%2BqzXHkjMgCokwTLR1oMjYG0RSFyVxn%2FdWEguyY%2BbgUjMGWTQz7rwE%2BXYTAnD0XaXwIUv0dKRQzLMKUukG8uc78kKdFRyDNM6Djgm7z3%2BJm&X-Amz-Signature=2e443368e9375f6e2921eaa52b483911e3d6df228bc3740acaafd075b885c467&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

