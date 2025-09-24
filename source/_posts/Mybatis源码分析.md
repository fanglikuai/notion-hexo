---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WK67PVG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T200054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNuvQgpEnQEhO9oUj8fAbffVofVJElcCBjt4YWNfugrAiAX2zobOJj5BBNq8IYByWBVyY1hFJLTPvNospcQScYc7yr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMuybhBTeRftw1V3pdKtwDprN%2FxWErAzvQKkOu0LUadl0xmDuryS5N%2F8ESc2EHGhH45ZqW96Y%2FrYWF3ToxISmH0O5QyO7el1U9t%2BTO0mDTVfbUp1ISZZXq9iSeSh4tEDVkNmSqP2MlixpCRwvpWMDuezDszeJnk0lFwh1tu%2BHUzWAIdlHWSImH3nDxcpWdqdT%2Ful%2FpPc8DgS%2BgHtzvGSXZDcMTAdjiY%2BMcRBH6CKYDkN5BNZR58fRx0gue90F1zZDun3fjysS8wqQ9hSpcJwwnYDrIX9tEmcdnDBZOnVFD%2BKdA3MVtN2IsIH4%2F2g0L9Y%2BIhxuCiELZ6IRWRy1INQmSvWYcKW8AFytVDrN4XlR7TS9Da5vVXVcJrWeUGIGlpT%2FJoSCi%2FrvchZxVzrBMKFGx18m%2FaWwww%2BIpTi3Php6qTV%2BWMZXSgh559cAnZnN%2FN7y%2F%2BxLkZUe31gZuwKivNXOB09gNT10k0kF2aA6EeF2vJqTSrGkvVV0NsFIEAIDTX9FL1rpqFYPBCcrVzyrjngthPqD8n%2F3zn3Dc3ot786AX3ZMbkCpbFW%2BJphmG5bP6SSLm4x16Z82t%2FZGnK8wnORHzBRCVa%2FUr76KKL4SRo6dlcptBSyN20aJRV5vh8G%2FMdI4R1K6owWJhIOlhiAsw7P3QxgY6pgFm7LtjPeByGMm9S2rBg4vok6IoA5uiuDL8Iw%2F%2FEsQDH4qzGSfxO7pLP9tG5GRDP07ATxjGbF%2BkOVeNgfyvLB8IS%2Fb9yjB%2FNDDcPWz7CLrqJ%2BDIwmT9NZozrAhYe6KstoF%2FTsQ89IhDjXXqbu9l%2FS8gfMn0L%2FqmpZ7Oro4s%2FjyBi01qgZN6AosBFe%2B8fjnO8iaTLTHkpebhNPQ2zCGKwt24uCnr%2BeCi&X-Amz-Signature=37601ba0df1a74704ac415bb1ca3414768b0ee87b7ae02231bbe13a5f41fc7d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

