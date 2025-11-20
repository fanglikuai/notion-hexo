---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664Z4O6DGS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIFQpleJ7nVFFm7JgkVHXvc%2BtLUJtB9xHT4crvHiydJ1UAiEAtFoBammZzzj94%2BOsrN0RYv2GfmD03Zk6o%2FnHl1TVs6UqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcydLyCfsH5oZbpJSrcA23CPKW5WIsEWTK%2BYfmk6yg%2FWV4oG36bNWjJfLiAfGIMCnCpdu2bWGrBpRtoNgNpSEKczR%2Fj37EEUeSMWSnwI86Fus9Sm8yidG1%2BPojNaELfRHberjqP400F13crGvILFPeTQ6nTUn%2FZbcvgPWLB9vZvUKCaBiTjsk4TQ0Pff%2FyVEVJqcCFp%2BKxPohW0Rh8e7%2Fy5knsX88wcNpZkiMseKRaEtdHV%2FSYxTsPP6Kj0bEL1nMmTQiD%2F9pgVZ3JuRbJ90Zl5jfhDkuSEuwW9MOwKpSyKQpIeMCo9g6DY7D%2F2qldrC7skmULvXruvRr4tBtEnYeInP7g%2BstTfqRaole%2BsDqaiEKvICdtpG6xHmrDm0qZC0rOSpArmZ37OD%2FeS4%2Bzl%2BbU3llq%2FMwqdRo2Zur7Go34yYZPqNbBzx6h7ZUb12CN%2F1T9zz1wUneyKyxvdO7yAMAee%2ByVmAhMySDI97wbVOdLuq%2BgOX3QDpS4qzxDW3vTpEUa5K1tmrV1wza3VpnZQi1mE9Kkw5sdxMB7rVyYI5iFdi%2Fq5dfjxo6grnbgOEXZf7ma4c4Qug1D08kAKSmNnJiUXYCTLuVnXxojZZ61m5QEPnb%2BrXUANkBybMchSqj%2BxyeS%2Fewe1KXVxsDseMImK%2FMgGOqUBHqN8gG9aWSLwG0ugdBg1Rp6lRFoKEjQVirYlRN%2FjGjjgUqvPUwcdy%2BO51o5VVmFWylAbmyrznas8JxhNKk8VMr20oIar%2FpzHLQJixq3bZMGMEIqeUGK6rMFKoX81xKKnGTVo9Ub51At8ezkzgAShgexI%2FS70WFgS0ZhiymmNxNKcdGJ%2F0hZObhdo4%2BeVy8X4VA6GRl1ors8ayDLDAiCTVmvdGM03&X-Amz-Signature=d58e91453e7e45385500d399dca87c6c90f45c2b6e3280c53b6596a03b0bf0ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

