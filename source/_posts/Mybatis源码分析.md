---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EUMGBNQ%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbBauEZfW2l%2F%2BT4b5tpzejmfv9iE07bEH%2BtSK8sVh0fAiEA0BPyAjqo%2FS6FTVGFC01yw2eGtjt3QI8cyd%2BxkfrbLPAqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCi90KLW3cTFtkeZZyrcA99h0u%2FmykVTzZtFR4XQLlG30N5VP8mpGNy92oi0rmiJnsTciCdTd4YQw50Wl5vTwoTPqawmF71gBK7lvQ%2FA91Vkd%2Bx9bWoT4OT1qnRBHV2p4fk7HU5WRXTNVvSa8dClYYpn3ud6TDAHc3L2jZyOcLle5TgPwzDzsCWzj79DMEFkU%2BjEXdn9oNiPgcUsGmKyVrgGJaDr15c3iGBpqww0iCVsZnOkP0Ludo5yf%2FjM8no%2BUe7VsaC%2FezwP%2FJ0G9zvIzM%2FpC%2FGNXQzae4Fuak4mKINDCqAknNpMGoravUA5DnDTRaVvh%2FE%2F09RMf1d01h0HUSRyqmsdwRC55p7AOrncbu6uPTXbrewsNsXT2eQCmD2y29IsYDQONUPvyDj2IVoZ2a0rSyC%2F8gOr%2BLqRu5b44rk4JJVXZUwNI5kUkntvTfA6bk4FcU8AhVrVDTXgwVp%2FJe00FsHgyv3I0uQDnGdDu1Tx3H3LCihytwzPmW3xxVaiZh%2FqFBf0n45Nclu8H38ewez703a1zBPqRG13zOFVRlWRdf71hNfWr9yZhFsxWAe1uQyJiNLzuB0ycQE1NgB30baGS0EbrNItLbqydk7mxezplXDOL%2FRRgFzC4OvJT9BUPNxDN42c5JwB8wbNMJugs8gGOqUBIJVZtbonBgg9uzfPvk78RxUxSa0EhtHRJ5NLe0Z6avNhOkbZRN8VQhN2P1qsgUmdyj%2B92qtLI3qtEhogrXFDRksELfCza9qFWWMwUFbm%2BbanDhd4jPJxRtI7Y5Qb%2FDQ51GnfB0awks0X6Rts%2FZtwYBnDmBn75HAhvVOd5b4AWRfv4XWAjMVK8ObBPwYaO9fJlPf3fxXgi8Cur2oTYI8ceOCQw1Kz&X-Amz-Signature=8028ce8c9bda0a320cdc62eddc916e025c9f83fe6bf839d49793482b7dc4fbf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

