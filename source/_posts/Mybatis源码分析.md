---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLHXQEZP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIBC4iDynz3tTV8HF3ySumZWDi48goJW8WVFTtZoz9tNvAiEA3FuFF4dUP%2F2yCSBJNmqdJpPDk9rVvBYmFh0b8Hlq2Ugq%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDGDa2TG4Dodi1qXiKyrcA1BXVwrBlZSSUnuk3JhOktqpNoxEKvzxwX4ZUGqId9InGQ74sG8aHJr362X6%2FurIWnXv4oSxU7IhyGS9EGRcDFgu0U%2BAVuSkB9Yr3z216OwQDBnvh%2F32DGb5l0zswDTLqGj4IP7KXIMtC5ikmc969gjPQzVKPAvUjeJ6hBl0ey5vyQV9HsgmZErVevQij0t%2F%2FTHOBn6XjLEVRJhxd31uTS4gTkZJuARqTnWQw1waJi4p5NWt7arwf1MBAjVH6VFvwPrFz954REZUowKkXaqzUcpbarptKGR%2Fl%2FVa5jLnnS0uDn4HoYp1IDXjPRRUyWIf9rY5RaVGvTQkntV77kLrlW6%2BaGKY7MKOOSbLq8wocIFRXBoCwJdGM3Vcju3Caq7OSvwi8%2FZHZvwPwsZgmCSl0pxQXa4S%2BmhM5QMsHgcY8LOrzZLDUZWhXGBkNHZAzPkc872GdYTBMxWQ4Onb7H%2F4x3hGANB15gPs7Y9rkl41YkNSFde9rEt2UZtVubP7bDluvL2OwMJDfK4aJLNqutcp0%2Fys1h%2FBI61ONk%2BfX3RUpeSzZXOaJLl7fhFUAQqvWIYaA%2Fx7mCn%2B%2Bn%2BXyGn0DqJA2bZj2kVl%2BvkAl%2FC35LdcpU4dNawD1vjEPl2NlsSqMJec1cgGOqUBZDkGzliIhxUWX8zYZ4bD7jNwglCriUpg%2BNKlEZUfURb9yb9zsnQp2q4OvGRlFzBz6VHwq%2BWiFGRGVNVvDxMddPl74sCY53OYB55ckqn7pPQnGdy4PTUqu4iWCV3%2FTsIpitph2%2BWykQkik%2FGHwXop8lBSQ%2BnBIO1%2B%2BXqzs1G5DGXnXFMNw40qNTMiPe7jLIhgAVs%2FwAUaWp%2FnC8gE4IeOjuRqdREC&X-Amz-Signature=023022b0f9445a15b737c1b9db9ec7d4f226732af1c090f084d2ccc4b1be5a18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

