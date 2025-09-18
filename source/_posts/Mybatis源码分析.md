---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKH56EXX%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIAiSxzsK%2FGhkO3oha3aI0giaptCG0AcexDXiCPS8LY8XAiEA%2B6nx4MGGugSsV%2BP1YRkQhARTE9MCwL2eoDjG62PhNr4qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDItIzoIT1wP1yNJzFyrcAywu1%2FfjFyEsk8EpwnBJe19FUZQEVoaxsJXOFoy5Yn2ONBw0GgR1wh909kqf5WJH40YUwISGsttioBhOql2nTtevHMXY0xafhRCrWM28HCGHwA0MRT1sok2%2F2UgvpQi0%2BnNfx8WJ%2Fm4suo7xngQIKjkjn5ZbHqIlk%2FxHNp3vttVwc3g42%2BAaOWf2cFxtJTHWXQPP0xbOj3voEPm3cOnmADTULu71bxy2aVQWqK5%2FaI91R%2B7Ct4PLmz2R1wngTjIRKMyo4TEjnopNaaaz4A0X8hzQOq6PrSQP5nvKP5dxX%2B%2FO4pozTKJlxPymfA7seG9ONXUJTFyiMtRYHsLDcarcVgy%2BTQxQD5KAlnMzg112DYcik%2FRAAQINM3DRpI1L77P4rVEJjhYdTWAlk2O3YSHjE4kvOET%2BzCaVxreDG1jOPEDNFeWr1D2AB4TxEBwkXrxHaw4Dl4u8on44AlPhGhRovQs4YAjHTsHxtI%2BZjJglOroLVuvg6%2FMgQEXMHhB55T0FTLn7C7QfV%2B5nLN9Cpuob8GVRSi2gs3TXJsKvxze8mJELHFHcCzo8cLduIVFHhNwsjnaZfx6Akm50sqHMoMjc%2F5dFpMdUEJsayYt5TaFjrHw4Gh8mRS7rNH0bWqQCMOO5rsYGOqUB2CsKjyn9E9orfBbseViAcyxbzkJqgs7qglw1eDpRu1Tv06nc2dtWvYcBkF4tDuv71yoYBdFbgpkscgZwHZ%2FpOk3DPZzPNfKq%2FUDWWQrfszuv3U48yQIUpMrtW%2BzoxkfCWOiRWHFPRq9m5fWo%2BVftNeIvQa0NUv3h2cvpFRbcCavx9MsApvvwBsjf6OF8mY5GRYehEYgk1o1p8B2Ec85u6HS9jyXS&X-Amz-Signature=dafcb32e2c195a7e2d722b6558bc81b0c754d7a541f3465e287d2a42a1de2ded&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

