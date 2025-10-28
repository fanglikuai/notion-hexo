---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663AG7JXZQ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDcvkWUjWd71vMqs79xzvHaHaAlzJSkbftdQmzHFqbqKAIhALBWb%2BZAVFH9YD6%2FsWVAHR8ZXjZK0MBxB5NhSS4fGARuKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhM4nfrm2EsnVPbVgq3AOSHK2MeEuA1QvNwUXS0m2IXhO%2FWlBR%2BNyunWo%2BAduNxd2shLCYOQwnN6Ize6DfxP3bT4A%2FOQFu4dUVhiDQYw0XvblfA2lKBSX5dBZT0N%2BQFeASkiMJyZt7q%2BjvgMOTHJqUn7qjsQIayHXe2EESM%2B%2F0%2BbFZKxmVU%2FyXr4xk7KuVF%2B1Dqx%2FhZhm33vdxdNSEr783483%2FVgcN3q%2BnaOtvISiKxnZkDpw7UqU0JyREFQGikn5eg2BOLuHM2Ae1WAmXW28GUOI0MBAOy7tGX9kZ0dmJXPb4%2Bgg6rrpLtA9AsNJ9hJUUHZS8uomXv2JyY6HNyq3Wrt32sjZNmf1TRrrLxg84Uj7PvcFJl%2BlYqszrEHB7JfncICF%2FfdKWrKo3ItvPKNPru4mykxU3kbhLdn8ScoX4FOpEKNtUpxW%2BFXbTVd0%2FO39ANtvzWBlAOgPkjzn7VcwEV26B4AlTBukY1l%2Fc33ayBF6wo70kUqpD2ed4pcAUY0iJ6R0%2BBnmxBCBzNEo%2FYLQUYtnUplQSfdWTGzWKRAXTR%2FYdDqjFwmk5Aiy8lbaNAgjTLLdybzff1G78NqPPi0FyOYxgnO5ki7cSGEAN9dtHEc2oaAxwpuXmCA6L93iqNZMpR5Cy80vEKdgomzDasIHIBjqkAX0gAfVsbmvvvyhOK2r%2F69hmyT4xkxGdv71I2TFRHlaFXF6M8m%2Fesd2RbNiN%2F0K21VefsJL%2BdM4y9M1gtrlmr2CJurw1wcbEbnXURzJfOcipQ%2FYg%2F01ryx3VhQNQHTxWDZW6jChlsvHyTnzLRSV%2FreR4GCaUNnvjKOEnm5nEgN6SjrwI%2FYVOtW9KB7u9bsQPZBksUexh77sEO1hV1t499lPmO9Zt&X-Amz-Signature=bc8f3168670ab385b058e25d7b92bf9960521f181e90af79290acb86ea23727b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

