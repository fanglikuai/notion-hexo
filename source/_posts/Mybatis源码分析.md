---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KUFMSMM%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4HaM2J3hV%2BO2nLz4UAa7ULqi1kOJRBwKPGcPwO8HnIAIhAN3VxvG6Lzh%2BC51Mxvrm6I%2FMSncKPGonKaw3PvnIB%2BsrKv8DCG4QABoMNjM3NDIzMTgzODA1Igwn2EHTaXk3RFGi4eEq3AMaZXwZGECLuM5UHA8%2FLvqPG6mHPPh5FZqLBAbqREgw%2Ff2WaThSaMqnn9TO2ojEnsN7qd5RTBfePJFq%2FcPtOiAlfwA5wweY3I%2FGtN7Iu2V%2FcbiKYWliab4oA0ZxipRoSI6fS%2Bfj%2BuDicxiNvoo6nAniLIhggxxPrTpm85NXPSUxQnchkh8Dqh7f%2B2UZrr5HVyPkbBPu6rbA0n3%2Ba04bRZC9Oc6bbDMqMHJ57evzHyLlIpaJmrIifQtfcd8G%2FhciURua0wGD0ZWCQ%2BOq68oCwxk16ym0U0bYGFzcsvrbZqxer1PK%2FJzOJmvqRiK5XdtNgHhg8XTeVp294I6JhRoaGTHS9UkV4hW4jkX4q4XNhzJwqfI4uJsvEaAK3dSKpTOIoLbcqAwoOGLNWRXdxASEni3LZDTBxkc1qxGLDfidKlIzT158ODOh4YR9Y4DfSXnsQqdOcWqbjN1h1iN0jm0eA4EAAxGIfUVEo0r736d%2BzB6xv7Yh0E0pX8jf%2FtthcDlnwbjr0yeWlTT%2B8UZJCiNT7FOAyB5Pax2cJBhmYWhcilkA2M7hXTj%2BVrcYDgohhdYzIsAqGnZMdHpY14N8RcXTENjaNJKLKvLh%2F52UTXl4mgaOOyeJqDx01CBbz7kzGDCeiKbIBjqkAcBpiJizr0WJ%2FCY5Wsx3HcRpMcQ28NqxTPUTaO1O38N5I96S8pSohf23ksWRaVg6sizMAXjaFKRsJlYSduxZcQ72VyGgU9Ij8E7nzOhGcTnUHox1947r0HLj3Sya9oev0rhc5K5EGGIgOJvxU8vP837AEM0aGPCaiodGRq0rN5blDdSNk3mZZGqfG%2FmDu9YKts2pIIJ%2Byn6cNsyQ2rNphVgPm79k&X-Amz-Signature=fbf9a89a350d79cc77e7d0fa49f772972076c45b2b94d6f43e6a771c0fe5e7cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

