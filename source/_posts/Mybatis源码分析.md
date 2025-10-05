---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3NHE2HW%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6jMcCFSvgbqAgP7eXfPa48zaOlO8BmFXhEoxj7nYKQAiAkRthYEwXW9XtiiP8TgM7qZrEqPRpvWpLByNf8G8RE8Sr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMqbeyB4Hj1T61GGaOKtwDxC5GbXvnNBh%2BcFF1qFmPxdM7zRd7FuGE8Z64Wxi%2BXh2zPZgjm9XMPY87TKlOF1OjUpzV4GaiPKrfC252I4CQb9mD1IAGr956UypyECXm5vNv3RxWEHMcUc2CdN79qHAglCwe5xCAOYESGdXsGeoOhXmzw05rA%2FutlD1rymHn4dpSMziRbtbqJCXNVirJsVcj5HQvy%2FbphdU4DTV2TBQQ7Dfq3oTwLeVXgJZvpb5iI6sU6Up13KoK0NNuNyoiC964d9p3oOVLKyselVGRBA6BVNnh70e7zNg86pJ%2BiegzwQyYZO0m4fYVTX%2F89XpjWwOYS3j4q%2FkT0uNH7rHcrPJMETVXiSehXGNrMs7Ts82zlsrFX%2Fs2bsmRUoFkwupBRyOY8qeFctDIHL9RzV4tkqOqPO5WuQV03Z%2FajktULZI%2BfgWfPUuAoLmy96D76qbb2cnL5Rer5XnoiqXhyp4HYOLHzceHVGkeT78pGAxXoSq%2FviIGIfCaLkCwdyXjXXEwBRVKxOB%2F%2BTDUghq9nip9xo9t9T%2F1ItGPzMh4UqbHrGyaRh2lXjBeOQGQlVCJnUmh8aduqjRKFTLpxSt0nnnAkrZPPNQQfahmzBgBuazmDZru60%2BNCkcxNCk086BdEhYwzpyJxwY6pgGW%2BYhgdeNBpChZj0bC7s7w%2FEfbrqwsjXHlIXBTP3Aeb2iT1xFFGa31nxORWvPSJ%2B25durEGaZZkuWSbKiWe5CMDc%2FZUiqP9ic%2FlbF6uhZGthgObvKc23VoLefVnjLLQc0AuSfIOQTtpORDpWAQ8nexn3A4DWw2JuKye5p9aSFbhkjtoFOzGKKsRMtR3kEnF3Qu85w2SwKRITGxoyL6CNibK5I1aGkv&X-Amz-Signature=9804ff2793a9a43f6eeee8d8905bd281eeac9236974ee12189b5b9e932c5c62d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

