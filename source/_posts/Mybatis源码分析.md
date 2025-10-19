---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5HQ2SCA%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIE5PWwUeyezTY1qEO8hvl5AwDt2lThh5NgbDWIQgc%2FiFAiAV9AQ8StsUVHZnwdusRn%2FqP6RYJqW4mfufPPOTFSBQayqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMq8MxMNs%2BcJYl13RRKtwDeSVsnYqo6wLNZjL0T8NQUaRe5tdNXG%2BQH91rh%2FkEAmV0pyXQ%2F4eaa9h1AZQgiSEqNUyK0AttOYBjVAu7cHHXAFmapwCYGi05eIfKbI9Ng4m7eY357q7UPJaJ3675zt%2FJspjYRNEgDwLTOuiZ6aigzhQoVAXytaV0ywtvDpjAxUmgC0ozOzzTetZCY5EAzs%2BEFGoMradZMxNsA%2F4XboUus2NUzmWtehmL1VhGC%2FYyJQDLsMGVPxmAiIbdEQr8%2F1TVYYRsym4zM914X5vrXyVAe82VcEhJ2yVnxOyUExrSTBLzQjw6MsIVS9KU8qMXRZqNsaSzHrZJi0XUe%2F5HNlkRPxEBeFSu8%2Bgb9ZILFmq%2FD4%2FRMRvIQGcylRKtWbxnTzf4OfD%2Btlwsx1E9OoAti%2BSulq9vtNyW37gx8LIR0jgDTZdxGdg5FgsbGqUZH9Whij5UYU6pFLWm2doFYOoY3aXHRjKzICx2NyBQkWACZYaSaslfibYh7n3gz7i85bVsTkDa9QlE1ljbMDrd1gtOOHEhX5TopMykXO7D1b%2FV1gOE%2B%2B0FSXQlknyZvH%2B%2FrY7ce4klNuVHZth6%2B9Ecr7zWlEou3KDcty34f6N9yPYekuY1tPEZ1DdOGbv22BmmOi8wheXQxwY6pgGibbSmXrCacGGPhTg9o%2BLotmFu%2ByS9%2BAWnmOHjwrAH39HzFy4uucqFFUOw6%2F%2BAOMVX9whrxg%2B8i62iDMH%2BLlMfzctQrIiMiX%2FEj3A0zcpNF%2BsTbI1zu5sV79bza7n7kVnqTJ33MP6RXbf5zGhGAy9v9S1yFQNzGLtUfQD6%2BoOpTA%2FcwsXKhmagvyr7Vx%2F%2BwbEVPU3E19mc4pqqUF5y1G2w%2FACHcRjT&X-Amz-Signature=f80b7f016fbe05bda4b6d8d2d1228ee21486f9db0ab213a5972174a16bb26b91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

