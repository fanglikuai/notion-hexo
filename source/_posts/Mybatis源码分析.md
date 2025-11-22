---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NSYQ2EI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIG5jncR42BtGlAmUKbnpMu0RfdOU4CFR0iHPSdwDMg9EAiEA39jAA6qN61j8MXyf7Hdg%2Bm%2FOGNXLpGodt2%2Fh%2BbBZEx8q%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDO6uGh7cJ7ZFuxXadircA0lU88mgZUDs7TpTJ4n3UwhyTh3%2BQElNDnuabModbGEnsZv%2Foom5OWxv52AM%2BPjolsbdvSjjLwb2SjAKAzkdChG7rHFvM3iM2bu4VppOsToqGI8Palwzej5FSu10WJb0Hb1Qwqu8Gyyv3bE94p%2FnwjmU2Lk4ek8ixyMnEevlBQCzA%2Fkm3NuQwJug3T%2FMafzKJnr8jYrgZsH2JRRYRCyWuFlNaYDO5shDSIV76Z8Nh3jHeDPTYt4Jn36tp0jYnI7NSbMOb4xc0Mk%2FrU7SDFsVQVhk4eAFSHArAMhzEpjvoVggap62tk1sMQ5OtGL6xPuovdXSoOcg15mihcuTMjDpIR6CejRdixCpA4R1IJcN7KR91BGnyGZrVXZFZA2jD1PFrM4XuDeoHivEELx%2FOuIubDXa7LZVY01AqDeJnag7w1VDggKuMVfPrda7myU03aeq7xWp0vTSP7FuL0XCWkm4nQKKM3h0KOrJ5kOrcqTMirm2pqR6cPugw6VMc7ZLkbQvnpZuSirPVWRJi0djbhkikBCkxB1xd4sWbCtQZykJlYo88evhmHhhiAO4OmEleXVrezxZ25aiet%2BDeiGlsuODBJYF9c2KnLDo7exkrKLE79EeTu0zd1FKkIKkt4dXMPLAhckGOqUBEL2EPe9d4UyhKoAlMsRorQVQNYgqtTgPZNeOAmC2IitD34aGJejJkldakkv6n1Sl5XJXwQ%2BzyXZCbcgsl2tSNSnigsw%2BlSXSTuPgzYyHNpiuJ9iVtkH%2FziF3zT%2BcH4Acc%2Fg9K2Ejan6vBDumzzQ9z0v3CNJU5O9z7Y8NeLyRZDq4p7KU5QX71l3mc7tIVSKTH59gcv1pEcdwO0OkYKaOG4gtTwDO&X-Amz-Signature=5db94e5493c907f645cc5bc59402343ea8696f41fb343e8d8480eba8d4503c45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

