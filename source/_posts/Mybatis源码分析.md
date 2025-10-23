---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGD4WAPD%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEMqgnRzSeFT4DZmm4Ws6yoCZgB6u%2BUNA%2BOyeZAsscxLAiAk%2BbTrb2fF7YrJcXZWaJfHa8dqy42%2BMq1QD6bfWfUqiCr%2FAwg9EAAaDDYzNzQyMzE4MzgwNSIMDsL7EAh3PzNE6AqtKtwDvE4Z0DzcbgDTirmN%2BcyNvmvZn8D478hpv3p5%2BYqkBa6V69T9v6jyk7Cl9nrz9QC2NoNit%2FFDdnoKxbKcPCLIa99UnkPZr6ymFlz3zxR4g7bPgLkKliUkvG5JjgJm%2F5U%2BqHiinqSa2SV7E2BJ2hvCDYF9JB0FMC3pr41PXZM%2FuHkr3gE0ub0KXr7iFyTTHNehEuFRgQamJzHCmb6%2FjuQxYEdnxVYJI6Ce4J%2BjyV%2FjrDuV3MpaLHOxdbXbpsqT46rIEc7jTrhhPifbVYsKIwjsgq2BtqZBEmUSBNr%2Bt18L9p5MTmUpqmaOOydttNIS49U6A7pOowEiMsioGCIoVDBxlXGOEzotpb4ZuQy6epFwLJ%2FU9%2FuUw1on4gOywmgmubZfzagNibMQE0LLATjcDaqDQ4ccSgVCmRVBgkjT9Pu%2B70A3qG6554yHWTsnxVqqldSn9csfFUU8pBs6jVWvmiT%2B1uRvS9F%2BsUJsUOySUSBbq77Ff5ZOheekcLm%2FjQXoSN09asDgSEfmf9FXhjLMo3zyTStkPSGU8z0gDim83pRdFGqsavc%2B1VtHMha1AgM%2BmuE1GT4N1Hrr0rE9hISUR%2BLA%2Fs4kyzaQ6JOQaF4QFqZ7h6poWpp6RQwsTJSzy2Qw%2BcDmxwY6pgErirBbN9xjfm0Pbcn8QkWj%2BF1%2BLvSCj4CkCq%2F4xCEr9acRCEoMw3MuWDfszp1mlpZEKkwWd5K28fLHSRNeEmHXJufzJkYvtGBA1v%2F1cu83hMQSw8l596Jd%2Fuojr2kOHHmC%2FekzjjvTin2wwA5xFbxsjfKjwhfVCtd1cdBP%2BGsOhCfUGvcqzL%2Bvi5ewUm0HMhIPLOO%2Fv%2Fi%2FWV%2F371wU%2FZJP0DYTf5n1&X-Amz-Signature=cbba629cb3c93524ea4d3104d4e7e5ce05b23d5f0eb9c4ddf67889b77f05f156&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

