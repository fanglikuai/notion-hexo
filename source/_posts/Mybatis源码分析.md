---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UBSVVRMC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGgD%2F5HPmUlfXcEn%2FbeSkg9Vvq%2F2LLjIK54HYhSBV83QIhAOnmZ1w7%2Bh84pWicvHxEeVyaUHQPIjBAKp2bdoGYpNMYKv8DCEQQABoMNjM3NDIzMTgzODA1IgwP%2FBZCmXtFF3AKmDMq3AMMhBxgZ2seESBKKZ%2BjXul6HfWdAuhBcFh5XTjS0wGUKCL1d0xQfDH0Jm2S567Pk2M9hJUJXv1wcMCrmkE5b2mNaUV3Rt6UduZNB6qi8Ha1w4j9hkR7BcEBf85XO3uhaLnl5pGWlT9AVEY9MEhTnahLoMTvrtaM5Gu%2FkmGoHxsCwMiDV3FRYBRdHVUSzEFedkr10PLeQ2DUHGXRNv15GjSNB27f60gNMAfEBFqGX7U%2BltGMSIW%2F8udB59Rg75bkVz3ZMpMa2quz6Cqu8HThvwmIcK9qgAK4HudSOmmwPhikMXdoAX8ZftyLG5KZcW8yPyCIXP9nM7H5%2BFyv1Y73mFvW2WnNgcWaJoUEnT6nkPwvt%2FXccu%2FwALdzN7ps6vY6jerdgLXfQXWfo0Qk31vwGR%2FrzyHS9GBxfeOiE34QlIfiM0ZdQ4VJUnlTiE78ZkO6wsVmT4Wv9sBrHXEnBeDErSfb6MeyvzvehaMqxpaM3Mov2rxLVUB4pEXKpC5tHYSL%2F2Z1ScK%2BbIp0Su2DGNdDgcVEl6mbCZAw555vKGIKSuHi1qjElpjScshylk58QNFG7rY2C0pXRcnSBTCN5wLngCebPYmP2HTO0%2BKFqED1yFOln3qF4IRVbQ5fWwBaDjD8pbPHBjqkAZ%2BA1FalGNhvK3467mOlWnU4CpHGhOzReI2vXnA8ve0oIkZkSGEXpPYHFZaHRVflOLhJaQ1Ys3kvIrotifkwN6GeUSpfpcFIMKxXwpOBIfvoDhkrVUiQ2WX94FbEHBi4F6rgjqKObM9ntMVZUs3HO9hpOZJPftynRRmwTUSnUBJjMq8xSugBFjifkx9q%2FKLW9xPsYspq1mcg%2FHx53%2B2XULpdGNtJ&X-Amz-Signature=7421a9cde7291451682a13b96d1bfa58113b75a798a1a2127048ee581373489a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

