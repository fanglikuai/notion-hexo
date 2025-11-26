---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVVNWANL%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn%2BXAJqvbPwWV%2Fmk4skdY66zZAq7QnHFaU8pW4vZXVQwIgXR3fuASADqC7%2FGp4ifUus%2FIT%2FHnQUN1J6noDyuwwyxgq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDJqOAWvKuBcgaRAqtircA9G1OwqMQnL0ERYvpmOubbPmNxfiWNBVK1eXt%2FGrBU98SR66bEjS9OmuoIsdM9QWQRy7N05tz4lieZtg7SeaAq48MjyD6Y8E9ve62o5PymQeu1CBIO5QAE6dPeWQdF0XOmSLnVC1UjvW%2BpO67V4pNk2JA%2BTNv02kICEwjp31bLqCw2vyADT1iJAfCiX2X7B%2FZ6i1Q6xmlnVC%2FUgKOczzsVsWI%2B0tz9opupslnJrrnbbHW68GlSnavG4bKICptfop%2Bn6EJDE3GpbCCKASZUdlQXLTUrs1ZuXChcwDXO194uf32WZAuJHscbBIcn1BnCCSHuPX0%2Fze5WF%2BBAJ5z6XrbWMDzZVXm1HinhVRE31YPPZ6DnEGRDKUSXbA%2BvIyMC5Om%2FA1u4TBVDfouW92GeC3XlYKhZglfGqhTGBjuW4z5LwmjO7kRQCNSvI9JYaEW2gWrEQGABYpiJMCaFKpSrXzolzAe2Ta1YBDmSwtkqj%2FwOMM4%2BQKTzOufA%2FoQQrT4WR2RJ1Sw4xbOwyh3is53BEcaxJFo8mMuuWop0%2BDbP3MIPwaVDYLM6GHREwpVu9itAnqyO80BA0Nkrb6oP6AkVeM4Q4tjW67H3Agcy%2BBCILOMzeo1pcwgqvoa%2Fx%2BzNO%2BMLTqmckGOqUBNfQpDz3D33zOdnuQ6U6UssqLrRBCZW9WI7G%2FW6A0TJogE03LW%2FORvcvzzJ4g%2FuR9XwoZ5F5KdTImzvuTWQ8VIpyVs5kwdqtse2EmO2NIL7l1drsLk7Y8Es%2F8auWrvT4FstSR8txtdq7vyuOmEZZIWtHVyscmaZlK%2F900aDB8BSUzZ6dO5ixJfn79GNa1GkLmvXVEaWC81kqd4y1VrGhWybBgWfTW&X-Amz-Signature=9fd8ddf56675688ccff7272a260e939fdadeac41d892fd0d8d68de6cf767a2e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

