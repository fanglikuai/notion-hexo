---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWLDZ3VY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQC8OBK8Rl6rzrmd0nARsPgylXzkRep25U%2FBn4wOaQSDnwIhAJnpYvSEo8yE2Gv%2F3f%2Fp4pHq%2Fvr30CYQpIbY5%2FiCMAcFKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0pD%2BqSDRPqRSMW6wq3AOx0ljdgiuzzb2guxpcLLlB%2FuXi74ZrRZZ0m7o4YQwhatoXDPYd56yagcQ56v8xJasrfboAFc6kVwjYT6E%2Byw17idEac%2BGzlec81dhxOiTcZR00TUgWAN4a%2FIXvfvWnsa2F0PIsakPC5ejpFa3oOwN3WP%2BVJsI0rO8qcG0H8hyzEa8iauIvEJFrM8RoyOaoIRhEUcqyHF%2FAR4txg9UstbfJs%2BteAtGzY9yYONPsrmXw55T3i012PGPqU2TonyGTomyMfPO%2FF2sVgvB%2BV0TQ9T4jv981sq3EdJiKYBfkmS9aeC8E4ghi9CiPh0lRV2zaLkPcbacuNW2E8PIUvQSEKG9FjFcQwADnUxyOwP9cTTdwe%2Fgoanwj3gWvQERESlcxK4M9QDZeL%2Bhy11bpPIBK7DVXxqzW6eZMPlfTKQm62aa%2FE9PLfiV8F31hW8jBD7GpCEDPi%2FjTtN1B9ht%2Fw08tTsQRkudFIuMrvtlyEt8%2BnX94iZ3znr19zgGP5Imktq48XDL%2BevbY3txvbHHhutMwTf84v0FAvvqfuDhz1cnD6thOZ7uFcRe7UN3U6nmo%2FcCkGNgaaoq49uU5vr6RXEs0%2BgJ8taRkve5Av8zfUqCz3i52ZZF2DOB9zOYXW9bERDC9gcPIBjqkAQiNV8rdNF6zWYWp1XJusHnZ%2BjyXUTPSF3X02u%2BByH6pL3XnWa3sMUwUpM7gwBNnKj6yBi3SsaH9JI9bM1Z9YpVOo2DJuRy7np8Q0ZFzWupu1nrAIg4ier8w3LNRImCIo7TxzeGdjJBrphUhIQxMnJMVgNOknNvyrIHKaWfn3CjfHDUoAxKtl9C2EPkDQwhiSWggD2m3fA5abF7DcQHBsCY%2BJkBA&X-Amz-Signature=45626f9fc7d9373d24d6f030a4e37e1cba0eea981b4fa85098f78a3ef629d5e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

