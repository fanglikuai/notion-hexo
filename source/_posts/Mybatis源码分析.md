---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTWXVNZL%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGYpnGst8h5PexKKnFCVdFca4ijajzcJ6if9K0rNistQIhALQoZmgJ74S1ooiI6risX%2BOhBNhtIvtZ1BlcBvXIrm7qKv8DCFYQABoMNjM3NDIzMTgzODA1Igwl9YfwvGVK%2BRLs1Hwq3APnFakDmKmfmxjntL4GXrktHzb9y%2BXa4A1Dy3PIrquxT4X5RUXoELv51K9w8SX0xAPccHXB6kIn%2BTfzIf%2Bhvauq58cIbTmZrWZLeVrqAjDOWXREd5Kb2kZgWEuPvdoO4p2lKBqPPrKuJWSQnxx5OWxVKfOUh42lZv6bcGkelQQSCfZCAtQjfjdbz%2B%2BtNZ%2FVKQLUJsJlaGZshAP7doJ%2BwRTNIu5cZ%2BlWvV28tn2c%2BYE2L5U7gVHqxSijY3pmiUUL1nld0RdFZQpES5a%2BqLaducB6s2SWOYSCZ%2F7fCvwi3vjK98O8JtXDECIzEy0EmRyA0d5MPz%2FcrdAjhQwSh8TQX%2Fd8wC1BzZhbD9%2Fok5YW44S%2ByuaN%2BtwITQ9D8%2B6c8GRGCcsJ0XcvMM0S8qPJWcSrf6e0OOAC%2Fw06ped%2BiX5cog0KRg9ESlrTIxrsb5tNYD5yGFx6AeK%2BBxE%2BhswwLiHCxnvM8FdVjZMtouivell0qmdqozg5QtjQ9%2B5LNH5koNWnyCpeKxkDbhM1cDHs5vaSGPPJ3XGDgVaMCdvT5Q730IlXAYGhKTVS1cJwEWrzh5knSXOtgCiynJyDt7iiNNOGvpyd%2FXkZY8xkqEGQTLNZaKKkhiqo5xBHg2u%2F8r2TLzDWopHJBjqkAUTmN5om0OT8C2rpnu6OlQ%2B5vKL%2BFuy72kU4y8ys3kAE8jgQMbbrH9uecYrnki%2FdAhDQoWh279op5FAOYjK0Zg%2F41Gix3ocWBhUXO7m%2FatYNXR0z91tys87Gy6pdnkRRCRH1UZd5VMaqpfIaasdhKiP2eqa79zQMGp0zsS%2BMue2jkAmlk%2FiThx7zjxKrf71dvVssMOZQQynLIzwmgFwhbpCXbtrq&X-Amz-Signature=570060d5a21c0e2bf8799fea383247cbb4650e7041dae70389016575323c2375&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

