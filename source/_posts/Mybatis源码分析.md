---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5VXZDQR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCfpsiUA5ZAj3gcbo2lO9cyQVfT0BUimo4nRl1TkAH4rAIgFc5QOKACv5aQDLbDVE22FVtlK7AcXUjI0Dtal91%2FKuwqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVGf8Z78q2OUTHzmCrcAzm9IMkoJY%2BkstwnW9muJ%2Bia8gpnRCVBzdGEsHtjoLVXSU0HunF2PUK5Ex8kEDAzwq5xnib7f2iMsw5doQkTstl%2BpueCjJMzGreVWJGEGRDBjSO5sn6WA5yVLFGj1SWNsPOajFoX1sHeOrvzEmMm9AXMQZpeTCt9tJfW78A8stowJMk2PS3f9Nx8qXDG0SXbm7y0uge0kMCbBOglyxXIeWxVGDe71tUV5UfCF44paWLS9Ia9kEjgdT4%2FfKCdrJeKkZb8lBWtlin2v9Wxjn%2BTGiouHyO1VQTbPWaimY%2Frd2DJ6sokwuTrWEtcMD%2FPdVusL2V97V2BFJDVNbhqRwGpsPPVo0rkW8U%2BPFLTVB%2FZ5ErfRLvc3oSatM0PGk3gstpHU8pwod9qXdTIYmc3nnkUoHD5UFzcS2iEYIyjiGTqC%2FA95CtBlmXhW75Wk4YG52m%2FzvrU8UgBXWsVU7LxEkDJA2AmJUeLyE%2Fk1x3nROQ2gpWLrNn1HgtzQAvKzVuYwJg2AGcD%2B9CqNpXq8d9W%2FnuS0TVG%2B2p3diP8BZrNz3pxaOQl8rhnLGpTTbdvehzqqZcd%2B3vSfDHLLFzcdS8qhpeAY3nRIi4t4y61A%2Ba2VpVK2QZTemXkKm7qV6vWIjnWMI66hMgGOqUBPLkS7aiJGp7rRGzljnVXzJCk%2BBpgZRZ%2Fmay2EyJP3i9QCg5Ej%2BFL8GEIVGJu3db3sjRn2%2FSGa7nOhCTIR02JMbd2HoYcu54l%2BG5cN%2Bn0ATAhS0l5xRccxToBGEvox6y2YI3%2BojHEUt76kuLGxyU24cJK%2BnlB0AcD4Pqs8sRlOFIR5TnhaGGQZegewJUU6itIujsYLQXZV8JYqZFakxcaNnwPWBIe&X-Amz-Signature=6f5ef3bf56c071e784b141fad3616f6302485dc0bde268df4dfce77d8e67c540&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

