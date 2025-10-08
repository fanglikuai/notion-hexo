---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WSBZ6E2%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIEny1jsBG87kOYSO3GSyvY1ce1CiS94IWwMwy2u7pMyDAiEA4jDAMOrT6zcKCz3h5TdAiReBD8ZSAS4qIGPgVLsvk4EqiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOm8PqUZnc7uXMQYbSrcAw3SmhzpdcO4rNH1wv%2Fz0x1yYiJ%2FRm1x9SyToQplO4rHq8spAxh%2FGyTcrfrPZPZRa9d9qYKkC8e8TuXVmZeThM7lZhwSg%2B9YN3HVo2JUg16wGu%2Fv1K%2F70hIrm6XAfDBTB0PfxWexRT1YcsNTdfpK5tB9IFs%2B82xSYTbGnbj3ILVZ4%2FyASchQ08q0UOLRErKZTYps6TvfI5YZXAU2GUlX833vdck5A9xdvGJ4KOIMga8JW31NB7WcJIFnMj%2FujRtQVaaX6QWmxnhr1jwXnDqjp%2BD4R6m4vhbClW%2F5qv318aFjgytai5q9vKBbTP7snjEouYNPPQEw8sbz9hD1HIuqcKA1t88CccTwleChirNrpp0W30pti2yuVGtr43h8SjbdQg2fwe0Jj453jeYXkNt8x4bxc%2FijUGAEOelzbKTns5BDVC9CfobU5Mu%2FhXvzWNT7u6wXYeBEImHRlUfTe0NuAeNWthmEVsLtSXjsc98%2BqHeu1OxTybBQjktuf7H%2BeHLdEzsy7OBGQCTt5oby4YNcD9KPAc2yQZn5DZC2RSbLlNSfQE4wB9niu8gx7%2BnWHwE6QNF7fkNe7bqO0N6nWI5sfwBF4%2F4zZqOkTJ2jTtBzAx15MnIYw2phncwtvozGMKPOl8cGOqUB3Hp%2BXTmv2ux1%2FJHs1YDdSEBI1f%2FHr8ti9RZf%2FcEIAeGnAE119l1Ar1Irku2Ww2NRPoHR8HWc6KCNErXaEVJD9cQ%2BvxxEFiZrg5mpCODSwa9IS8wjo0D37tc9iLdkLEnGI3K5Dz9IMFp%2FbTM9LHiMQ6HM9CP%2FmqMlkwHSCU%2B%2FTBSq9WmUybmWRP5uuH2Zbfpj1w4v6Z0YcE73QGsnDU26OghUMRJf&X-Amz-Signature=5be98abd05e3181b4c21e232787c9dac1536e9e806bcf1fbacb6f4d901d48bb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

