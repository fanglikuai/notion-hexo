---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVUN7G7X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOswfpNhUXYoLUN2x7HhP%2BFr2d%2FiuZB2yKm3n7YflXygIgEqD7cfpT%2BUu07xc1vDDoEVziJdITXu178bAMLmYCmygqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6Ib5cc%2F2EFPhBbqyrcAxWKJAFWFz0rBQl%2BadK30Lvw9kdrT%2BIRwr6SqoUqbPU5rq21cxxwv6L2Djg5pgy8IvtDPqNmAyK84NDPQs%2B%2BVV47UqafuM%2FURLfMgoUkaE0c%2F66V12WJtWOGVBGBdc88UwOUJQ6g%2FTMIxYOzMO2AT4bjoP9cD1AAfHrAG3T8SgmGGRjqZ8uB33PtpdIgDuiRtDfSPFVKtA0s31zWEalQdRwxyS2eBR8GLc%2B3X7iF1Fd1bx9k7hAkbq4IywRnvqge5VguO7ZgFoOAX0f%2F9k8OPmdjaM5E0Q%2BI61tTgpWU4nLrknN%2BL9HZ%2BUd4doDPKVcJqTrwCqCn1Xl9c9viiU0D1qe5XqnC2PyktdEIk2dpPJRpA0EKfrPPKeT70OsmwpO3dOYItSzdYbG4Pag1dWqedtiG2B72CEQzO3GrBqQO%2BD1gejNjksnGsGy0LtTNr52VZLbvJFMK7wDL6UNmAatM5TXfkCaLUV8PmmqqvaDStYjuQ2Zo%2BsVPCvPUWZVgA%2BvnYr9ak2%2FNifH8w2LS813TV7ad8VEA6Zptf7JwXs5eBaqEtTObDpScBlFFA6robRbmNK03Ugi8qR%2BgqBKI6Fh4UzTlfMXTrUHcH36E36XnIoavYnGyPV7v6A1G2aiYMKja%2BscGOqUB9RzySpvxwdpVblIQXXDpwUqPrqq8scxgGyonjoCl%2F4jT5IwdAY7xe3z3TUSr1zDo6%2Fv1VKvNviWS%2BR7vehj4ub8w2Uy4AVkM3Xfxnx%2FV%2B0o3ER%2BTAUKa1NRVYsAoDc1XuPPh8zSe6gZqm1oIOwChYpcKa4Umvg9r0jIO67e%2BFMtKkEkIA1Ni5VQNd76gZiJkUokNLpB%2F2yQ80UA%2F3%2BhtB6poflD9&X-Amz-Signature=e530dfed71cbdbfa7b90cf1ced9792cacf9e3dbbdcc76fced961a2fc01dae740&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

