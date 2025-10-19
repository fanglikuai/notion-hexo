---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7HWVKXN%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T160037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCnRTSqvHg7aJqxFTGD%2BlFhJAlPsGKuIhlU6W%2BwYZziQgIhAOndnz3kEcyugRk%2ByrCh27jFmNVQCV05lhjXNxpru%2BubKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzJ4BC4GqPEVgDpzH4q3APM41ajo8IyQpwi5m1IhnbAbfBgG6GfTq%2F7l15g8sJz6HpOFzimF9F5ABr75vFztF1DDzjQ1Fxd%2Fhkh11nsPOeDnljo6Y3%2F%2BsgJu4HZgwoRiYXgCxb9XP4%2ByCosOp3slyLTaAoH8lpWMJ4jsHTLmS%2F8PomJ0fQaJZTJ9aUiYKWK63HXRlhEv4G5TCdRhNLjB4%2BDJihCIBmrqIrlIgae5SB%2FzdJupUyD%2BgpiBzkdt696vf3SyXTJTYsraKIMrO94UdplI50fJzBRP5EagALBpKpnxxrFt%2B%2BabduJBUIesffJdqBNuPTET%2FZZ0yfeNbz6lGa3VeRTnafKowg5tLh2ifE%2BLSnOFTO1x4Pb6o1Ltx6Ur3u1lE14IpEua9H4vsfLT1tBCnYAHiHfoytb0%2BwSTDMHEw9ZUXGrG8Rr24h0SixpXNMSF5txirlqgAQ80ZwABcrx9KaR6nCKqCMUORh89xjoGJkpYup6Il8dTxyO4zUBNLWNUJao99rKkCctP1hk4difAA9j2D1tVHSvznfpLoaI2IyJkqnKZHQ4D2kgigjG5UsFCbT2uJGF28r6MBNmykkqN13iwt2XzIjEFxnB0ObEEVdpdQJ%2FGQ1emtk%2F6jarTq9lGpLaEj2Enj%2FWNTCXndPHBjqkAe06u6L9MlQD7fXVkMI3DBl3rr3dWQCpq64m4z2Q0ber7MWrISQDwcXeMBJTa6URcYGlKZBDMuTWPrP5I2BtQrLK5lVTWNMLwUOPu%2BEwxAdl5s4G049sU5LckyJd09fa1d23ROeOAVwbRybx8bTrW%2FLnddeJpDg2ER3TsxZQcqJkV57avSz5Ipjj%2FTLYBlsayi%2FzmgrDpHgeslk8BbB2SMlmJJRp&X-Amz-Signature=331a0e03f9adf25f003dea586103a58a8d208a54d78b5a34ebfb20c2500f1f39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

