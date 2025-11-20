---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZR3HQVTW%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJGMEQCIG7Y4K%2FrCbd7%2FWtoPmSKTTZ3PL6xobOiM3GcdZQbsom5AiAXO3ft6%2FpSxLTqdVB%2Fqum%2BBMTBhdbrBhRZqpyzmZTV%2FSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4%2FvTnIZ0%2FGfblPGzKtwDlKW0298hiyArUsRI9ABelEFevOJBZkH%2FM4076yzKF9g1Wp62cZgiB2qMVlCR%2Fib55ElYuJKcxtN5ln7CmzXLfuWavEB1bpzfJ7MDSLHvS1iFm6ioxUK7N%2FyQ%2BkIfj0J6o5tTn6u0vFlbtoCZLp%2BZ%2FernzdRItTulB%2BdR4OiUDT6P2WmnPDQjOPI2h8WLopRU%2FDVKqiwlEfueeu8%2B%2BQM%2FrqkOcbP5M42vEsP%2FmwZjp0NtVU90fkI282kTMx1ZH2MItnNlZCIvMfFCOuYiwS94H9%2B2I2pM46MPcLlXVPFE6D98gUhQZ%2FoFLkH6bXbwq7wvoqxD4EI76MUVlKMUjk4hsjZlen43tuaQ215rqAq%2Biv5G3o7FvtfXfvTUn%2FXTItPNRTlHBgMIdkiR%2BG4WysqYdJkiuI5VqPnTIMfK8p8xRLdbRgR1m2jrXnFpfpDKmN4hKoFbsCnOyV3K3giO8OGyze74qrA6Uz9dIDQdWKtNQQEW69p2Z1OPe2ztc66jaX%2BwziPCEL2qPx3aexeBfmuqF25NwAYAtgUMy4EGgYXLiDPM1doRDAaO4VFIxoY7bDL1G47uPAVQHOrTLpBHyHLFBH%2F5%2FDjntc3SeWoEzPdQheOYuzUtZZLTV5i5s%2F8wqLv6yAY6pgHU%2BJg54hT6mU7QRzlt5vyZlDHrvx1vdFKi4sglIoVR2Uca1FnbVtjDfp2cbLC34a8mDVMfZukJRfWdb6fD2IpC6tOghLXWO8mQn32Pjz4sA1f0RB2vO6lmNBnH8%2BUBDgf%2BnsxEx1OajCnb6rE8i77JNeRf2dzQoKOsoYjVzxeWOV5kkkYnHxM8edJbHl3PVwSMR810zJTFk0EdXHP%2BPu7H32HooK32&X-Amz-Signature=f38c32903f18a96d91d4999cfe12b3e0f4a29bf50ffe2ae4dc2bcbce828c9907&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

