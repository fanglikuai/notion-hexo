---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG2BE5NY%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T030045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEDz5g53aE3aFZMHZ5ox%2FCqppoN%2ByBIH7HuGaFWepUHdAiEAw4iUBBmaOBZlWwdyRErQoCED%2Bj2I966drJ2YA8w4ByYq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDKGo721q2pJjZxjHJircAw1Q5MCfLFKC7fpb8WjUNDE8zMrr%2FxASYkuLNF4bpdrBUOmoljCKZMHCcFHNmETy%2Ff45RtA4LglVpwzfRyybeZvQAeFUOUDh8WbH3LUm9dL%2F5S0GDmfOWv62yfGvRMmy8x3FZe%2FzrJfitrMWQDAUbAfMPK%2B9KR4cT%2FR2EI2itrQ7oPMI43rCpwfR6X068LMhf1gfYOPTsEVSunAE4u3ErCxkn%2FX8EhaGkfKh%2FDMrDkK2Ydek531OCJt%2FTqhbjGzhp3nQx%2FFCqRdaWw4Rm5iV6f0fovQiT3M4mKnkjO7eRe9NsplPbUOlL459tyNT0EZ7c8TFTgE7FTDEszJ%2Bc8n4%2FvgzQ7JwVbI83yPQRISZkjB4Kz%2FQpHwNDwmFWKvfAFRTinuCI92SM3ByIh675gr%2BM%2F1%2FhAe1yZCfNQu20Hfs%2F9Ly%2B1xllGg1SPgXVfnvoTKS6scPA5j1K4KLv1HmHJKD8w5SLaucKuMMLooX5ohbnXtcmDLigdA8dCdf6bc7wMYSHtjLY8rRuC%2BZ5v06G1eJFG5D%2FJOtoLXz8I81nq9lx%2FRn2Rb8uFcnj5yOwF%2BWqRjZGxnKVM%2BJ4B1NVGVbZWzXTKb4qpihColug9Bno10xlKiUTifzthVY9D59O76LMPOmq8cGOqUBMo195yrEZsbDcYh8nH7JBPusZ1Td1xDvM%2Fpjow24ZKobdj75H7WfFEGk2qIJc49m6n5MqARYc0%2B1nMj5haEAdEnhSEupU6jsz1LDXHguAxVcVpRPi5gXbRs4mLU2D9bnS097rEqQAC4Rr27ZVd28Ow2WvYP6hGWMhJCmdsJvWwolW%2FYM4AX%2FOruDKTmTXPIeG9Tt%2B2umQKbBhNNi1qk8secF7tw1&X-Amz-Signature=c7c92b5145bc7f60af3f4f5d2b36e36bf7904808e6e51b8a04ddacec0bffd161&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

