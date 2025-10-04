---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YB42XXCR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9WNCD0EY1qhoQG2JQgSnb94M8vfzaJ3UPrbaEqN45EQIgWWuM2zmIsNeYW0o2%2FyGGxO70F4fGDlPZWaKAtUWAOz0q%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPob%2F%2FhSSVOCZYR60CrcAxouhVehJ9qYueh52OrAplZ%2F8w8Bztv%2BGgs90jq%2BFY0ny%2FZmuLY4RKlfVt6Z4O3wTvIfuSWD2vREN%2FMPDEuKr4XqAqYyHzIY6CEDBW2B3xt0QBj%2Fv72Ol4zgdVwYvLG4f1U8pJZIXsMBh68AhkHAPntU8VdSyqrYW4ofYjdYaV7rG%2F85zr6N%2F1DXBahaea43Sg050zuwWd1oF9UsRvQTE%2BG%2BmoisFWRSFku2I1i7SswmH1DR%2BeXPXauWRVKk4Q4jh61P9BOAPvB0BIVihL61sHATrrfjS14chqhy7W2yQXS2XKSkfxMEx3oskGTG9Du6Mq6WMm7ZMlv32DJDXw9%2FbRqGXOu5h9Haqd5316oSOVMq66Vb0h964TXNuhulZgRu4LZAUroPE%2B%2FE0EQ1emxBXTeb3Aolqrw%2FyMP1c1Y2ilm49TaKNDgTeltYctCdzT3tePzluXZKh0njcY9yuIrX7%2Bf%2FnbXWTuFJDUoKgKFKDTTswcXh2xhKiwxIfDpsfz9mQKlT2AEOBlaAmSnrWF8HamWyCi75AD0hh2tNxh8779032P55lM4ie1JJbo8HuIS9s3o0mu2ZaYoeFSG4gpMQgazON1A7yuPcjB%2Fyri30S2zv%2FnF6KAucmqthGLwnMN2vgccGOqUBe3OF0llrr%2BgR4dUsCPM1xaNrrhm6of9zUKPvqS3p4JMaOfKPiMnjis3%2F432syaTXZsRxAQhjvJtuXVFpXKjvg%2FrRHCTq5JlgEKKZ7lqTnmjE3pCC%2BSA1d92UVegxd9bR%2Fl4GJfZWjdJNdMMlesvO8CuWnv2TiWTK%2BGMAkcBc0x2jw9%2Brp12WpntgnHS%2FQPZmZJ6qvEstRZMWb9%2FhFmBsJzQPpDBb&X-Amz-Signature=388ee30288aac18ef07b58c565d5ffa1f490fbe9354e097946a8efaa07a2d53c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

