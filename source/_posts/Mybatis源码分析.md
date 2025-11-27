---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FIE3ZDH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3SrOjU5J0RtDnlNebSZ%2B83SIyIu5wO1NQmp7QuXy3HQIgICn%2FGh90HIHPI7ss3hV11jmcpGaN%2FPas8lWltBH1uGUqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC38LCWbMw88nf1A9SrcA4lyjW0bVQ8ldBG4veA22c%2BJNSQH%2BKW0t14eVuWZx1rcf85%2BM7jxWvPn9V8kX2y%2BLBBTi7p%2Fy99EhnvhWOeinPwX6opy78a8SQCrCNb3RKAPaLPKL4IaaHUm6wZ%2FyNJww%2B2ExoyhOqg1ghjLi4xDnjJ6lLAWRDA6GOZBfT4DM0outjKDPrXyp9%2BcvxFU%2FpS2MRXsspFe3U%2Fxni%2BCOn%2FuYDRoZiJuBI1YtL6fk3wbINZqhtsjULQDqF7rixxiODdZbHdkbepOTjE8O9p9289TdMmb9qOXaVQFHw9S9g94z1SVypADIXqWuJT6QKRnIjis3E4OHljfVkc70kjf5zZepSH%2Fv%2FIxYBsBnzsqBwyFPnjZab1Up81rFTqLLw6VqZ4rbYTUYEzixB8Rx1ppTMo9wXpevKV59qjZeAJnUQVcGbrV5oghF8UsscftIFiFV4rvxlYMCtfrQ0sPzMkP6MbJ1O6byixXTI8NW6xT%2B5%2BHGShD%2FbARu9aG9JzGcGaFh%2FRO70vDHuDpqZWOGm91nh8agCax0ifSsrNBuSrmakhopgrKOdfjPxeF3CXStwaNn32bnM%2BOedLBEsiSbxTw6wzwWhJcGKKzbmUlpo5nMGNevGiQPQJRU0rnCCVRVT57MMHFoskGOqUBcnEJgOwj0Xdfo9SKHW5%2FJncG2bcdydlVKQUkEg9NaN1V0NbxFUx9gVNcX2vPSTNxNuKoKZtEQvfSoES%2FFPp0ZboaSfV7l5JnDlXWPtYtdIIk4GRjH%2BErAUwOv73Mk7LQI1CObN9rLwVGdPCm0vLkTfQFiIJzR8YGGCs2OASYnVq7MiuRbaNZpzgfcvYkufijuqwCZ6eJQhUJU90ZeC9ERgSbf8IN&X-Amz-Signature=601500c73a22f12856fb168a749ed8741bbd299e3cb424da5ee428cea4dcc172&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

