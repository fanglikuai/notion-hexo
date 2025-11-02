---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTCJ7YN4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJIMEYCIQDFXc7STralWc4tOyf9i1i1GPcXi%2F1Sb1pKY9JgNLhQigIhAKfyksRoK7Y1Lefa%2FFP0i%2FJ5jLFunpAWqcQo7vMg7EqrKv8DCEQQABoMNjM3NDIzMTgzODA1Igy1tE54%2FcFRVzNp%2F74q3ANE7uiCOpRvnEFaBh18kFR5WTFVaTLNFsngXlxyVwynnJpOfEzqY92HTuUoJHEA%2BRd0lD9uCIqrjWj9cfFvjl4AtBjD6OaPPTLwKeYUBCnwptM5eo4c0WwNaflFaMgRpBir7YANezSHyL3%2BCujzKmgQya964%2B3Ly5aMPchuZXD8PfCdDVmaSW8%2BOAeHzTvDkFGxwxkCCUxXWpVDnorySW1avB8oqRsVQvHOPNlYlfkEI%2BYB9l7CNZ3CIKTB9rVGguwvQ2aVVrQoNaSA4xXqiMote47al1vwOeQ2ol3%2FMu6oDrRrSxAf92jwdmkls9i1OZE1M9%2BDXRtU0coZ%2F8mg8FEfqMC94%2FJiqEtvtZPIDBlmtMyX4oIg0pX7ZfhpCH1lBNjWtqYgYGeYZbhsUwMetKoJrLewkAF%2BoLr8jVDJKgr2Ml4CHqrRNLuBeuUztieeKcO2D6KmsvwauWvgptDooMzxUYP9C3hTAelvCldFsFNlt0a1tFwSeGIJpwGCUy7HwWhaA23EOrXSwt0GOxYOFKZIgUAV7zzw%2Bu4FSMBKVrx%2BzaA03Qq2BSDR1BUj%2B%2Bf9W%2FntAIa98D7Z9zZtQTAe1mMh%2FxJ7Juu0CpKDU16GkZTXqAkL00YUySt%2Bbw3EvzCI%2BZzIBjqkAefd5Gjowem1Lf%2Bcxl%2FS%2Bx%2BtpXrHlo5kR8ITvbb0ahzKWiO1%2Foep2FYE4vfbMxx%2BWm%2FbxBIrO1TQJiezeQHWy5S81HZ2NKUPXJjVmGReRUofYZhhjIlKe%2BHIsiFDihqK3MLYfI3o8ZWLPYfXIfRRnGeFU6%2Bttlyx2e6ArgwOoZENj2UFlR1UUuHVewhN7rXntzjQ9dhHxdooEZki%2FFZDF5VRnqLx&X-Amz-Signature=ede0936088ea920d0f4f16f623aff6b73b82aeacf3945892ca241d76ff9356f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

