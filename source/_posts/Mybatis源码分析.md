---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSHWBTIY%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIQC0O62ONbckHLjOAsv%2F2ubXCg0J6szJhYgR03Jao3m2KwIgRRIVAVKp8LJv2G71voVvixxp7Y4L6lvhfaCRtHpv6jsqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEwDjMSgFwIfdwOz5SrcAxQERd6kmmif6OTdMCLW5kU0coHV8R6NWDB2EMtTk9CzjHHfVUAsE3gcDPvufLmuzitZLKVui%2FKMnAdIVAJg0LzAPaxDX%2FCnJLahA74L4Efxhhjsji4yizfzuPD9lXiAVFLFQjh3onm%2BZAw75tYDyRHwGSFrQ%2B7ZRZCUkYGP87LEyozMZqCLxTnGyLOPgS0P%2FxRxi6Bme%2Fx8yZka7VakrWLRXBH4ESw38GPtS15BUGeoblljRYsAKTuNYeybPqH4gWC2wXso79TOzGMpLFWlOGL%2F2ikXXpsbEACdKzh5ebiXgCfL%2BfFu%2BqgTlHsjpD2TRbTpsbHo5joSkBoR%2FWsuepS0RrFGwFHskD8gVNMYUaRds2nP2Tbr0txlx8nlMj0hvBXOccGzGlaIaB1CcVe3aLVnDGp1Ct0r5s0lThlJMjuJOGgrmhRcJWOSDZw8%2FVtJdWDm5mwnRS3lkmmf7eNAaUH1C73DgdoT5qw4pM%2BpPfiWvkR9Hercl9mKniMueBM0iYSHDaB3E4vb8idjhR8lzsNKUGHZ1I2kHScZvyrIcnyiU3brbkt%2FdBM6bJ%2FHLSEvQbbci8KbSSehTjdnRyNdjE5D1csDERSWzSyPiV0jLUA1YTXsXxjDVHEN9JsvMIva%2BMgGOqUBi4To9y3%2FjV%2B305F7Hv9QZTHEK%2BoXmTjsyqtntqtYn1R8B%2BYzc9kjRNRL6enympGGDL5%2BmDwI9JwX6Rh32PZqDCxvq7CFXxrmd63FzfC4e%2BFVa0khExlb%2B%2BVkExYes%2BqOr1eTC2CsgXHWPloUbHnsm2u377M5O3jdaygYOgfEy2sdgh6scCc3ZB0bCI%2Fem6Ma6z1YAqXxl73KeEqMpyIG%2BZLtlw24&X-Amz-Signature=55449069ca33f191581e7ef3d6fae4470944f25ec94f1db6ae53f4a8eb6e30ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

