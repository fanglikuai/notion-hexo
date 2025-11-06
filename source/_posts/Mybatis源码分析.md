---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNTJERUB%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDdeo5%2FH8z2hkrneqwSwRxj6j0mBh%2FZ1Il30VF2%2BD%2B9cAiEAhyf8MFgIqHI3DpP1bXERutrsniT8mhxOvEcP2SVvhHkqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuSMbSaH052%2BsgnzCrcA0OBNzz8bcPvZbSNojVMsmPTp9hGIrW42PK6X1B0XZ1%2FjxdSTxsfYT330P%2FZvSEAWxlXOjxsjVBfUib0CH%2BVl%2FMYfgvk642BCjdGz9ZFw4vwzvyQV8zK95pXHdvl3GgCQGwQ2Xhc7U8EzMVbMUOphKHntVWaSSvG%2F3VGwyvaFXq7PurvhGyNJ0OWcWz9auq%2BsMTLoIXtMpXy0NVgq%2Fyu7dX0NgxJjMgKgrgWevhhhVtNk9n%2BH0fn5w7HjW6G9fpfzyl5FrBlFDbe4eFfvfOUnt19Ck26rA%2B0Ea%2F9rRCzzq9dhvaRWncmAOSubZnN%2Fme8K4X%2B1Gf%2FQVKJyNO%2FXZ7AiQ7H1X3QVOc3Ae9e1Ga749jPSYUNM%2BplPBe418A%2FYkReHt%2FWBrPLH5lNHsBcQkGAib8kOyl3YkRJ0Y%2FMSTELcf2%2F5MbtLV0I28tvOOua5AYaKp6QIvppLMwOMaiFg%2FzgGo3Rcar2uK4fPu9X0NH416nS2hnhOndiYQDinBUm4QlyaWJ%2BLgiwvJ6vF4znInrKpNYMM9UhcW20xGh%2BIXN4pfmEIkVTxXVmIcASNQ4Wiy4HCV7Ss5ehBuovlpVJ1%2Fn%2BFF%2FuUpqL7JtxNvxU2DDOySgTio9JM9mDDW%2BduVy6MLnDs8gGOqUBlHaaa40nZWeePFa7ZTxJt47y7vPEHACvXKyOEKOqpZDCKWyqnR9jj3z8Q%2Fqml2454ARgXdoqa1w2BCOzhG2o0qcGqQR8i6DxNsUqeG4FFxxHoXFvoC41lTvauYuwZCFxam2xsz0HKFrq62OLFOBpyQ2rhfvkyfi27L1o3toROcVZdFI%2BNfg8kKpQIl%2BVN2ay%2Ft%2BcUqSlbveivJvCsyJOiX9%2BFrq%2F&X-Amz-Signature=264bf58ca22784a11aea6f1d1a8b3fae04faa5edd6042b388ddf33c15516b643&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

