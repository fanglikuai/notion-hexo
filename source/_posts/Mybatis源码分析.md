---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WU7NXWA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbB1mImacR%2BYp0gkpViz0FZQBKy9K1G3oT9xSU9FDeEQIhAK7dbRbwvl6u1BLDmiJitIhK5PtOFAqAGl3JRNwzU5BRKv8DCE8QABoMNjM3NDIzMTgzODA1IgzRA0ynzAV%2Bwbf%2BX8cq3APlfB6ERQefA3gx5CUHXS10H3g2O913XESGhOfwAc%2F7%2F9YFaEyQ78k8fvtf6bdSM%2FG1A5XhbwBYN46dmd%2FTHc3ZNJ4Ahat%2BGNbU5doBmXoZ%2BjPJmj0OX6Ol59wI2DiOzJ3Z%2BB4Ljn5PB5ng5EZaseI%2FP8WWqdM%2BUKFOjiUzRGvXkcmfHPiHCgrEuW%2FafHhBpFoEh%2BOFLyaaaUuYIAwjOM6F5xeICV0B%2Fw%2BT4m4q%2FATQK0OgJF53fw9JOsdP28efqppqaw5EAJ25Dgvhg%2B%2BqGkJzi4%2BJJfCMcBaFpZzCHraIG3Kkr%2BS7fFGPqdWP1ZjT2KtjKhr3%2Bbcajw8WyG2vN3rcs3CiKoPL3XBEqlUFmJydU94lIlSO4mXJtKxfoqTKmEq6fo1nTst49TFIA%2FXG4MuugTZpk%2BS76UQdshRA%2B1COgiB37pQERJmdQgK%2BrZKxFM4q56WkdrqkAwB9tMpXxZ5%2BzJe3eaWi7nFBxqeOAqbFa%2FWZLJRSEx%2BLbyeu%2FYd7u3k57dF2Syg18vygHujrBhbvXu7vNQgx2hYEDIC2rAOcmFVsH5FH%2BTrpblm06ijzb3xyIaNDN6kZlOaS2kXKWVBFq5fWWnioMf74BIm6rxmIS7efA10WRDmXScSE7TCbyOrHBjqkAea8Mdu92zGJtIk1TNdEw0ykgcQvI8JC%2FtUs3TNOayAGzXatKlelxlyj5nSVOpGzdsBIaiLAcI%2Be7v3gwOaFuGGHjiAvpOtqFI3PijtP6w7Q98GDSmN7G2zYUeOZNUaW7IJILs4Xe4Sjj0tUMYwAklvTieM8tUhXngTvUJ4Fq96d4r1wvjEr7wTpHQee9PMBIKN2E0t%2FJGFXBVNlDz56G370DyNe&X-Amz-Signature=59f124b1b78167d667e66495be4fdd3d52cfd93bd3d78a4566410964d6c62ee9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

