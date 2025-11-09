---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REKTUSYA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCfSw%2FHwShz8U%2BQLgPPNKzNAdlnBn6AEzl2gI7kEqT0fQIhAIs3jBu%2F0k%2FbxPCaZri0o2KkLKDFScedEbjYZeIY6OvHKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwVkJvrXxNVUoR4okUq3AO0KMTXLFisZIVTFMx1xOh73%2B2QwjZssR%2F%2FtuNgstvDU%2B8wd%2Bbir%2BHQLBNP9q%2B9oYvw8dqQwog0fpRHvkZtmAtIt3fejJmN7hTa0nOdXqCi3k%2Bv0xxY%2Bt3nfHwiD4g5gmP32KB5BdQQ5ZYovZ%2Fi1xwANeIiRWgESNbkEa1QKJEuhew53%2BfAKRVg%2FLx7g2AZ0HtROQtXsTPrrukZsi4tbufgrZXCXN19e7bsorqf%2FF3Ac0ryMuxbV5a6fd1t3isTWQwJsBFMSLe6vEvopQ46jJ%2FzTgiL0Xm2s9QqtOG2Mvk7f1Kq8JGbcbdSqTTlnv9gQrczNsTdU4U5xx3lgF%2Bt05evBFHO%2Fkjm8QcqiVBxTjWPAvCetiONgFZF0g5oqs2Rz22a56SupnVziyCf1fptd25Jol%2FPaz93Mq4m6z7PxDAPyA4OuZ82%2FKhLJ%2FrvNByRwT%2FdkzxOmpEI9okJMnNXAU1toU5MFlGNfpyo1Fdrl8dUxFcN%2FP8T%2Fud%2B8edqYl2w1BEzAnBrfVL9H3qU6JAXQw6ifxyDPrUHNY14R%2BDHjHyq3OpIJaUfQvNYOX8a2R2ffpkK90KwoU28jEJSSiIp0LwPayHc7wNdSKQ2Af3GVPD9PJOy70bt9ABETIBy6zDX7r%2FIBjqkAQTt3Nlfu2XvLMhFdvmeFpDvEQEDZoMeveFgWfM0CWA2aYAaDDWJsTgwe7fSMsvcWjGyMMDMSVeYURKQV6Wzz%2BRiRPQrSIh7UilKxYSCzgRHWU71MpOW3iCUym8Hx4HldlDh12igWCFkkiFzSVIKMKKxfXTeeXDg0KrOvocWXBAxOcPUmBLJriEL94GYRexmnfMmehy27U30E2WTSFGo86SA4vjV&X-Amz-Signature=8bd58db98d77b031a3044b08eeffc4b85da38468278a5405030f401cfa971831&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

