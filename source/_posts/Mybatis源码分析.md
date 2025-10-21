---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZSGLFYE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQDmwfW0d%2BjI3dTYXgtECfDlNX%2Fk5sLDlO%2BuqY4TC0WkmgIhAJ4IE3bnABWecfKKkyCc%2FzZRSKG%2BBpyWDYLjfltfTLW5Kv8DCBYQABoMNjM3NDIzMTgzODA1IgxObz8JREjfLsKGSDgq3AMHxTQukSlRLbdsXZUP645Z3v9vHnJw53tbf%2BUyVd9LGxvZEgqxe53NvrsjOElkTmaZYrN5W6RUonLEfYkU8lxz5qLJrAEVvX4hKzBGZGyH1xmRPUXybxc6yYy%2B7WFiA%2BkBNI4Epp%2B7jNgtgcI83vE6n386VUcBc8zI2kO3n%2Fa%2BnvfIm%2BBxdTpCiOfBCaqFN6UpfZEgEC73f7ZNxqCuImsKQUX24jDBmG%2F4WEZX07HTjvT2kIjEAOEGwEO3Lm%2FFD9xfGviw2xNNwwkgIlIZ1RcFt2tXAqVYU5klHARgIbnl6m03UzX6b4n4nd5Yc2uwb5a9o3tuVO%2BBoIgBTyR4QDrobd0N%2BSi4IIKhoRQRYugqW0NnkOFbpmGGhXJw8VItFuO9oI%2BgiFcfkES1b2vG16M%2FM%2BhX6CLCPLXtYcVJHO7ADt9o92Wjbqe9O4u7y%2F5sH08zq22BQawBwR7rsszwY5bmGkVn6Zwm03MM0UxcTlkoBG4Iiraf4SmDf1jCBMZV88NXcBBPuQgriNB8FUk1B63dJLuM1Ago7xZbxw6ppEpZnxiZX%2F0JroUI7jNNmPEH%2BBA1x2gtPRFWvebbnofynGfJ6E8t1S7YkCpTUlNt4i%2FdSHe0Z085Ok61%2FPNkHTCN%2BN3HBjqkASIBIHiMwwJ2Y%2Bq02MvCo210LtldMvDklP5nu%2BDD2kgelBXwmwS76KXr2btOzk3E3Syvc%2BRWGVmeiTpzaoVQAJqP8z5p7NkqJvMx2tbkuPL%2Bf%2FAKjIqgLeSE8UJuX8toZuPWBzvUazHTffvI0gFrww507uRKFXqXdZYiTqzIr08imzJDvHbo%2BzGLjvCgKHCCrQiAOn9AcZxCgR40G6OyG%2Bf8FPS9&X-Amz-Signature=a70c5551683e08bd8f71601bdd5f8ce869ef6e289306f5bc34b23ab88f58d8f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

