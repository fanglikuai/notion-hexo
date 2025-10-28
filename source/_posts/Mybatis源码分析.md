---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BGC6BVM%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T170103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIE3FYaBGpQlNuOBptzO7YhelyICgOl%2BGmvLO27Rdd92AAiBM6aY8iuEW%2BEn9jYMrzzScLiGaqHtr6iIPDEs9xlAGMiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsev2Mt3uTHUXxoiTKtwDoA3Jm5LujxchwOjVR4WZbjn97jWPdr%2FxgLIfH6iV%2FWK%2FF5hYQW2bTpIA5J0TsXOzHBobyFz4VnHpGCIYMZ10nHQpKyJ6lGY1oqMi411VRwsThKkHfimLb8M6WNYyZGTb68BYHDOVT7iA9g0LBvbaZkziWuy4srhJ38VsV40pUcH2W4sPuulsAm5k1jgfOcXkFFU3pLc0fjdUWO1Ga9yRHWcCw1RCwa%2Fsrty%2Fh2Mmkfs8n%2BNbBnV17TdoUmznc8bBFJ82mOYqyDPiQCF8MNs2OvzIw%2BVYqS%2BAegjbQEVZ8JCRGlH8xRysRsijdfP6xMf9%2BPsSPfTPwnBEVyPSq1eyH6BIUil6odRR4DrPWWkz9sdMDr3Oq3QL0%2BL2B%2BxOtWcyZWueYA1J5xAU0nIA7V7%2FMgLbA5MvDOwbJCNJ4%2BQq%2FPHARXtLJ6iLOW2p9UMVhuD7g3CNFHxyFq3gVIoT%2BSke1g9lf2LtbC%2Fu4kfcKlDyNoboo%2B1pNZMJHzsT0o7lQt%2BHkP%2B%2Fg7pNN4fjiNNLewU0elfeYofg0%2BKPFiXjDZfHQ7MtagjUTDX0DFNmXBd7O9g7ZsVeDnskGWNidptl3y8dQ%2BlwoOmfYGv%2FE2WaocS2eO6e9aOo2fkgxOB%2BnY8wgNCDyAY6pgHxEem%2Fo0okU2bbzf86bfuclfkvzbcE3qxAIzwM9do%2FkJxerYTjFysE4gbh8m79nW4zWNDj8ZCAje5dP0TVaahthfMzTTQw6v9gqTfkWegS9hFUEg5aQpmTSDQO0kGbk16Up6n4Z%2BLAfB7%2B%2Bsk3nWY8ln9Aep%2FK2T9gA3dC2lCN35316ffVsI%2B4URtO8GJFz6vE%2B5TgHfSYnFScIGf2d%2BbugD2Vge7n&X-Amz-Signature=753b9a335d68176b78b3524ee181128b13216efcdd3c01bc7782b3270e99b7e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

