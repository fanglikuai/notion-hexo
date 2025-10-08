---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636RHJIT4%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIBcLFJiMC4fiH%2FOKCgXc8Bm8Hycm2a%2FjS3AaOmMHr6JAAiEAngnrrbPJoVglCNPZnSwFi1vO0tgLIsEbhSnj59x2KzMqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHDRiiEc8GwWJ9RCJyrcA%2BnsK1PhLwNNfz3JXBUi1Evj7pe%2BgfBRCQexEyjOlm08MixPAuJXiGks3HqgUiGkEIvCTZhpfVkG9yaXoE7qmUXJZNTlnwh%2Ffy%2BnCi1plIJ38NDa4maLub5Oqw3YEgSmuc1oJFGqFjvH45%2FacGify2AjRdEhFWTzBwq%2Bsc3tfo3cu0K4NQk0UpgQfDC4BHBmt10OI2fqxo%2FRmtzwg0P5ngJbZkOe5bo%2FoJxotO70zuumDYD7U6iXES0bBLQsWMU6%2Fav%2BBTVxmulZegmk5QMwR6d%2BnCC1LK4jplr58RcPwk2v4AxeBUUL90zbJ4nAKajRZa3tsActH9zMpO%2BEa6oaOO%2F2%2FBMd%2FclW4D4SoZ9c%2BYp2QxdGHQbq5JPlFq3VIHjV9%2BqIIVQ69i3cvSnZWs%2B8q6u4JToElr5pny4leywGjDqJ%2BU3o6ZeLvRgReC4FD4gwYENbX5%2BtqzJ%2BO6KJ90zb99KC%2BNYy2%2FL5E8Dvh%2FnQb%2BtYqeRntLaLR4hngUawDTUHe28t%2FubrJFfBHAVSSBpm%2Fzn3yNKQHIQ10p84EgaxUlDI%2BdBzZpxjyEw%2F6Y%2FRmc%2F2oq5NDwCHKsNIIPEEyTWTXXzvcEYmfN%2BkKtCJqYb93G3m3PYxIjMmyKGtW3iAMIrqmMcGOqUBYdgmvHgXRL7LFUUX2bAPrAwL51C%2FxFbID1%2B16UYYiDD2xcxWeAzRwI6z07C%2FouhDgItXABVzrBkAvENgNVQNmnDWtuEh6f9PLLMazhCSXfHy%2FqCluim%2BXkDupZ1Fk4m8hG3eWBQfUrLDBgRmVHvzVXDIWwYEADJAl7%2B4lDXyFPckEgFnq7cHjPUEBYqwlxRKVyVM9Oja3%2FNDmRyR0g7F3x88pR5%2F&X-Amz-Signature=d8f8ed35ad4b62ae5239c11fb202f6a6688df09e73c87f34cedfc9719d77b11b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

