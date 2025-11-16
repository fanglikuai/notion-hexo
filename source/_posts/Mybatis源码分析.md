---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7VDMU7B%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqooXaI8hmtb85cX5I2N%2B0BxaXQNAPitHvnFRgPLDuvAIgHdqk8ZYBV5xgTDiSxQjQlqBSv0mWI3fIc4QwC9tguyoqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDORE%2Fz7YyQwTBpm3mSrcA%2FOx3QGfbG%2Fq1dV1Nm8%2FXYEu%2FDVeqbES2aZLKjKTOwS4xyiCH4bku%2FPKtdPThP%2BvHAKNv7b32nrSooh0ERl4ceSHgXksDqjNxTnFAKu8LKSVm8Gy%2FpfBByr8AR%2FrVeRsAMX7uwZhfXWCzj9Wb0NdCD6IqarE1R1kdiMBiq7lNFxsa0TymDjMGfE3MBIu%2FtlQ6y9iBW6LjKDa35vz8VpPRWh6HXRFTnXrGu26Dx2Ty5yRObr2SNI64iAmR1xg5p2YngJGXcpAR%2BAntu4Ewx%2Bkx9TtEQqBo6cpSvQRjtvw6JZPiMn4QmcbjTjKsWwDTreTVZ%2F8%2FP%2BYPLIzb%2Fi9fvPT71DNepsje27n4%2FT7pPyrnFnmmcE54CtjyKIo%2F1tTXebEQuFqx4r7YdrL536CzH%2F8Cf%2BlgmUgA06RTMc98mAjvNl8tsBewBl4X1dNHQXscxFUYtzIp5zJgux10mU41giJn9PoG9xVDKTVja7tKtAtqV88xeoojmFehrsZlOxaOavh0eVc2MXcPsP%2BnLe4ulFfs2J22grQZQBwi0e67I%2Fb5gfnrY%2BWrXhaCbq1i3MM%2BUJn609K%2F7vMLZ8EKI8iXqjCZ2WYuuxBnlIvOmzgqPNamD1ycx%2FeuWd2k8w1hmUqMJrP5MgGOqUBNv5hcho%2BI8G%2BaH3LI8rsvT7r7eJWZZdzS1rGZ9Ml75NwgfymsQ5Rdg7DvjgyOTauBqEWgawd%2BCVdD0ViGLBL7LNWlU91NVVtlCwricb48ee%2FUDvKhKCsQygbYZu7Vdt%2BUacc8p2ymYrBcEzrBlqHDv6pvlMRPPhYLOKEWRHIRk4iRXV5xHvcvhF3tLbdFsa2KrYdc%2Fl8Su4T%2FofZOFZMfvUmdmdA&X-Amz-Signature=83c5e3823a6c302e79d47345440b5294e23eae4752c66c674c9ffbe07706cdd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

