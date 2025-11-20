---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTRAEBY%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQCc5cXmVBhUrU7gNl2YmZU5AoXOqvAdyCYXnQ%2FLegTKZgIhAN434K7g%2FekByjzCjVdy6jPyKHq9UzgZ%2FLPf01y7I%2FwqKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxrSFpbEWTyTOMRpeMq3ANY8yUQFC00EfvdEnFKOlwl%2BJrhGnrMT2jVQenF5h%2BbOqY3t5PJta%2BvZjcjJ0quHCcbXewWvshh06005KC%2BTwdb18kRDgVHdssdfzBJqkS%2BZ6UhVIbyCy7x2Xttm5NcfqGuqRgk5teMpSJ%2B2GOiSt2KBf4ZdzsILQHcu3tj1Vl24bOYZzuZ86l9N0TraadWP9vrDwqsC7AdQ8yT%2BVlo7lwxqEmIacHSTsrfJMU0v%2BBRZkzGdE5CvkTpnLHz3ZbzmV8AjsqkT1Qn9BukiPwFoefeaRPKX6UeNee43cHGg%2BBkQE%2FRWkoeRC%2BaKoz4RipvRvK8Zz48X63zop%2BUH%2BziODl1YHJhztV6OztTN7H2kl9Tr0N9swZzBy4%2FHc%2FWnCF4OBJhD9Yd6clMtUDl9k%2BGeQ1%2BjlsP1xJLCr%2BuSypQa%2FLe8ZAE9ojAxrNRyODpNm2NHrsx7VYsx2PcMHiUPorXtKc9cMOZuT9sgWgsdLLnasez%2BjvsKMjSskCdQDKR4T5BNzFYvFEQLWE9Gz61sCxcbgN9EvqY94QJy64d7Ev4Y13eQLhEAM6C6r%2BDZwW2z01IMIxoDJhjVF2%2BUTyIIO8QLaeXG%2BHF1DljkdyWb6JrhI7T3bZZS8lUbxweTurS5TCk9%2FnIBjqkAbO0kHbullT0GxOJ6HNkl9owldm8XPHnGG4IGqQWAQSLpx8zm%2FXXKus9h3CLCJjLXLVOG51DcxMeLejzbaAUs7GJm5tky97tMn5aKR8V7ulz6E%2FDOhGtjqkB7QwkkVz58IGGSzlrZKyXJB4VJoARz05jznDE3zcAP1OLDDH9m8RiTOYR6oeTTGPc%2F%2FP4NW3%2F8Pe95%2BUyTw5cS1vOgkbocS8k%2FPgK&X-Amz-Signature=3aca0ff1a127a4aec6684246749689f78341936f229118099d3c304b442ea017&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

