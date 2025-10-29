---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665W7G5HKS%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIDMDqiTYHW6v7%2FaZlcjNgFGoI%2FgCXRpfTSocSbLb1HfxAiAT2ufECkkrwZKxCYEIisVjvdtQZ5Ea6tlkNy%2F%2FCatvACqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeAaaNnyqPzLk4YklKtwDEEZbzSDqme%2FJh40%2F31brK3m7AiTVOVaGJpgovKbzFmluqD%2F744nStMgrQ7epd%2BHKsPdlgazdfGUJlkVhb%2BLGO49jnDXWAhbjx3eCR4trrWvBZ%2FUswfYtHtgQKFxrcAB6zO3vrzhaI5KMgkPniGKNtVRlEVa3IYc7BRgE4yd73Is6ejF25NLQZHmiXmCur%2FRHR9af%2Fc8ZS6ZSSMV6dGeTQ0%2Fz2U12lJxKDSAeDyFcX59CGY5GTT6PGS9MkmYKyiNHq%2BI2a1ZoVSQW4E2DBKYWreIG04vYptqcHBB473KJsM%2BCiNxcNSNdL4X3ybTAULVRAjvHfrwqKiscV2wU40Sxf6lVZgInWxpmtqKvwbRGEaKNcQT1K4Oru4H1N0lCobvUPyqDGaEx6lIoGGhE64d9XpS%2FqnB6O5FRivnyGdvmmrDjPZ9R9UeDD1IMeZ3J%2BXSYOA82GLptcS%2FqO9mzEngiSTgYgKze8D0SZ9sYpUIUFEqvjTxFlSa4RNafpGuCLvgDawO6NnIblxKpJexxcx3vLKSWej%2B9RbeFyTKXMfuDAbSYUIbV3PrMzkXhkXrnyHSSYdwPaCLg9WYwIV7zlCjxZnk5a3oW2gpluUFHyaTWvWYJSVo9umiSprRVrQUw1%2BWGyAY6pgEMua1lgdjTHTQ7b1HgcmRiLO5OyiC%2BLuri888cw8VVk8PBqAinNr3Pq%2BCievtNo0nWN8danVRAUXxjyIaa0Eddq%2BQTLj7L8L7oSCRtdIaKCVsRVqaSgQ%2FIHTKFj5TJHlb%2B5y8dh%2FRnAtyv5Yx%2FtANu1YpohobSYQcmLxHLocG%2BfyKtpjDWoUgi6bcZmbD%2B5zrxOh8PMkdooXOxpZ6q93Z5TSQWNO4%2B&X-Amz-Signature=4068955485bf3e7665dc0a301bff0c091886255422f83ae2fbfbb0e00ea62be6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

