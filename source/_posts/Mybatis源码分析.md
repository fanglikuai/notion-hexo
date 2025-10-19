---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFHGNZUI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJIMEYCIQCgkKUHNjFxZesaljgk2KH0lIThdbQVteyvuZRrA3u69wIhAIxm3SV5I9UK7h20aTM7RYSqCKC%2BbXfQ76RH4MnysH74KogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz1ybjLXMLnOyarysIq3APNyKh9ET7jYD2lmFy6HG4PqlVZVn59hdj6C9fAWgQ%2FXZMvHWW2De7tIRAEKtdme%2Bb%2FIojeOg6xhD5jbqbHCNLOC3MCX9RseLW%2FTgbV8Rgu9Rw%2B5EyQw28eeb%2FlY%2B8zyvg%2FquJl9PsI9kFVJs9GdOvcprWUjrFhRRlWd%2FIC6wGHuvqietJFFx6DFoztRJQ5BCBP7Og434E1pTBI8XHvZHuBAgQE5UOza5YmGT1abrfzxFVs%2FptxmfQ8g%2FqkhlnPLrIWQ9YrQuZfVkqIHT%2FZhXyh6n32h0yLqDvHlqPrvPusASTSk60BRY1a%2FcPcY8V6IHVfubPf75PXWzeOmlVoTBra4vZm5BIHY170%2Bg5D2QL7%2BPoa2SEGe%2FCuaM8jsSlEx4zj9s8nmxqGR5j7TMsJxmT510tBLmMoEfpuaHXl3chn93syq%2FllfbiIOySuMVLzo5jSM%2Bugjwd%2FyuM2Y2jYbG3G0Fjzhmg70c1hzx3rz99J0njKyvRW2I1aLDARzcOxlKx%2BcdQgJQ997AYkR1ANxZaYFRZkBNK6%2FGgwV%2F%2ForGBg%2FL3LHkiCZtfqdntn%2BjCl%2FMwTXQiNEwZjZgAEM412RGR3XbIdQenEyJWb2P1%2B2bjqodPg2KaWDU5XpgqjJjDQ7tDHBjqkAQaBacW2AehgCA1pWmVM5qkDVWYvr5u0ydvc7rEpjBoYPshM1FPKHasGv6eGdxpiMeiJ4kfoSgI0FVoqC6sN%2BOR0W7VMqoMy6gId7DSHG3E6cplBr%2BjCNPBxmAzOysbLTptPkKY897aZMF6lvv2iIM54vnu06iGGnDit9933Q1XV6agFLN0LEpXrBcyUxwLU%2B1ovbk6vUyXI0jbosJEtV1yT7gMm&X-Amz-Signature=d13c5515c17429d2b3f227cd6b433017828a68e797ff5a7fb2372bb1d5b3d00d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

