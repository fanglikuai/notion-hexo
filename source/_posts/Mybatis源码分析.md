---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NQ7MCK7%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T110036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQCUUBJozGnm1fd2bdikHWDEWGT5Q%2B59eRNjLmtvZgJcMwIgXimAnVdRHrx2HwE2s%2BOGMmjlnyMrCOvm8L3W1ZglA9MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIOVysaWBZ0DUFQ8kSrcA1upcuR3vTKfe444KfVbh4z8ZZc15wHuSHpI7pwm60dtsmL9VRLbSNpRFq3hqj9wTKRDtFPCQPh%2F%2FHKH4Aw5N%2FoYvbFuGknCRcVXJ5g898Q3rbdlQu9LMU5EG8GDXKcduh3%2FQ%2FyfuBmFJYOvWetV9tHrIaCU21PEU8Mc3StBEgWuv6uKnWziUHEr33MpM6ovkpvfD2zb80ypzsYzL1JuJcWMRVchprO6aum5hjkuWu8gsP7nPkSW7v26fZoJKCgr4Rx4b5I4mtOM8R1A84wouRNLZJSGvlPr2Ss42mcAjrxr6TiBZP7FhiUtIFxEqk%2FYp0YlI6wCYEY6z2HNKB6jr5LBAjiZjo60VR3xMTfj8V7zjyl40i64dqGIVhuslvX9u2yTjnGaboj3TOfmO%2BdmIJUZmGaG4lck2LAO%2BrFQeFbWgNETZyrPLQvLuDa4LDGDRd1O%2FjscYBQyN0j8xqtVRZU9YRmMy9GbXGAllrV7uwumgCABbpdhHiehEKnhOW9uhY%2B%2F0%2BVwknFbW2q9ZA47JLAvxe7GIurkECtJTlmKCmVKoaCli930wJ0c0H8K4JWDM0xFpmG1Aq2BTicloENyi6X0xIlI1mt%2B5Da5d0725MuWUdOamvOv7unXkvrWMPOmgsgGOqUBoq%2FSrto9hWp6VXRPbm7QWLO1GxeMaae8eMWGCANBRMmahgg%2Fane5kWdD%2BNYgt4kqeunery6jrhFSs%2B295Zigd6a3b3WtycnLKXG3IP%2FpCqYyUIRf41%2Fen10Yx8wHyZMbO25fz4KXVx%2FvLW8M0pGukzCCbs0Pca4cxLWGu8WT5N98G%2BLcgho0EvxOyVVkFjA7O3kESFp1A9f41MAlnRAVqGmXXhid&X-Amz-Signature=00d95cbe72abad9443fc461b4ec7f1f1fe507445ebc26aa0bbf64a8e6d62fcc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

