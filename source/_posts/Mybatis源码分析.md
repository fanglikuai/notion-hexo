---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643GSYVCE%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHqkRjLkF%2BRmj%2FnRYiH%2FbpmLxXiOjg9f5JnkCT9VoJhvAiB5uUmf54vi5XXQlZcOK7JdiyRBiZXVnAJ7AUGE%2BHvplCqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsi%2FnELsbsuD4MxTVKtwDN%2F%2FzW1LTXJoC%2FqujJcrX5uLss2%2FEOGFCRRuVKSo9S8buhtD%2FEWUnIEDcgW7IUpWendSy1HFzCkDD8MtYTsPz%2B6s3iMDddrjPaa6pYDDsYoKp4BgZ6l%2F%2FvbVBHIZxJS3R6aQBc%2Fh7cfdEL9%2FnVmRY2u%2FkX1vsIZE6w379hNlhV5jyoyJ9LS4IP9SIChWnUoBwhDsxcez2eIq2p2sW0gmQ2K%2BjuuaWaHmEDgRVc205mGunmvda5E7Ha%2FQc4LyzXThdVAE1MqbHurTqdVwUjf00IVfMTHA1li9PE9%2FOzkpcrKEaHZbmi5QGErRXVrcNz0OCZjnoW%2BPWHWtbXjKpNBRLo2Ea5rWXtyyEM%2BnulGF9t72LUwwCqb1YjkKx%2FAy4Ac0O7lQcfmMVCPdmrCaENHOhFsA8MFFYlQGvrIdoKc%2BFvACcnGy5BKC41GAefnVVoeCUPYOH6fzkATbIGzKrT9sSdMAiZk3msll8YGsHb3woMJ9Z4d489VRbAuQrrI%2FafdQnmW0DCUPHD8AyCjHJFTmFBAPnTrzoD9c90okgUOVikBNIL4hvnWdo01CU9N8PqHHfgZLOUz6T%2F9aqMl9ziPzKN%2Ff%2FyG6NZUPRBC4j5ZQFnAOgIp14guy4B2pdKVYwl93vyAY6pgFtofXoNCXqSNp%2BZCejm8tjUXYWmAQE1GQ5vSLcFTanf999beZEy0uTKwzftjLl7lS1L%2F1nR8LCMmOnQXVebGZMR1OYL8TNyTY6k4ppWdeIpiLsuwgGKAjrs07UQ%2Fh03gUFZ2EuyvJwkFMvFmDLS71z%2FXJKtx2%2FFDhjQ%2FnM8VwrU5dtJt1eEsXDagVixMhq%2Fiz0AKbIfu8BNXbtfl9%2BQtcMo9ES7UfN&X-Amz-Signature=9209e3d81d4419c2a9f9f72e4cdfba574051382e32b81cad3d9d716683bffbd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

