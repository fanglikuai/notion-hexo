---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JOKITTF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSJMGCcfvO%2FvS6wOmGza%2BZX2dQwD7pHTTraCho2WBLtAiEAxstcx6s8LHRqF5oITWrneLkxvV4iZKeLDJh%2BRM61WGkq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDBZppnuKVwwuFzttMSrcA3rOaJqks7MTKFuiZHgUP3e3iURI%2Bl1RZ14z2YSYNnRSESOph4aJZ1vrVKKyoiDgS4A2UROsldW9F5bqJAcJLEfZ7%2FDsqIXLvO%2FZgHIv%2F8mEtZ9yF1lt9yK64oMlHKW18t6lQbmXJRuA%2FV09UdHWKOQDmBH%2F7mQEDSDAd8ISuJrF48XzAZ5xjjgx4v62exqV45t3IrBj28ZOFKPbvCTq%2BF4rtclWPLPlzgMQdwA04k666u7l4IQw8zWQeVKBg37ry3Jr1UWRdLkvPPLbsRBFenzQpkacIxsCuHjt5D9T8IB47veTepfoy%2BVfmVD4tlN8SOBVo8zG4Dc1Mvyy99nEpLdLonT7TbveV1H%2FtRxoBARkA8Gsdxn8z1lajzUS%2FDYN2bPDy5RXstpuoeAvMPfQzM0XTAn1BlcsG%2BnNGZKlSD8FNN8eMjVZn9KN3mgKhU%2FdrYB5KRYusDzw5wdlNgmtGS2mIAUxLK1%2FlJocl014ArTO1rG8LyXvZQ0GowS1V7a%2F7%2BJDdlt8SWfgZeKDCHNynKkQgmXXg8aylJwXDp5UEKamlxakPbdjh9f2MaSOhEH9xvqp%2Btd0mbZLzCM1eKy8OaGRtE9x4U%2BF93gFffoiWJK1oGU9a%2FH%2Fh%2Fvcr1CsML3R2cgGOqUBTUR72DN0dx4PfgELwMeVh%2FjQ%2BhjcnSE61BeU3Fvhn9O01HZ12nKD7Dg1E5TDv0Xf3Z1gVYajsbOWvvWcUaWUn7yWco57UsCOy%2BAA95zXK%2FpoAq4qPqGW7ibEOwYS1%2FP%2Fl9Y%2B8lomI2aU5PhbClV%2B3d%2FC99fj56lSDuEU6XGyjGWDUH1F9cT2xTcyLHsdA0HrwN4QeNhPRAecTqNAQATAp%2FYdEcV0&X-Amz-Signature=1ab5a3393592b6801b394a84447fa2daaccdb415e74dbb89bfa5d2334c9e8238&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

