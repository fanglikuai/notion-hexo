---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFAZZ5TT%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIHfNnwsVvnLglcXBC9ZOW7Q3aRlteTTAQubWrci9i%2FN4AiEAj38M%2BQaYQRLVywLKZzxmchkQkIAXui3mv%2FSGc7tp478q%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDIDPzLXd%2BXAiXumi9ircAwSmOvYFryQKAd0vmLA47s0kbi47KlnR6glf9zf6WulD0%2BqptMliGyFNMrjDfha%2BpQr1eHTQx5Zf1oot9g1LSenYWmUcXdo54PtUMKL38rbzQq1qgnq0SCrfUanzZ5RYXUn0OoXDIbZOHX2GV8%2FxRrAOtJT0bube4QdhNQXuZafVsrN95x98SSPGqH2gPVGkz6fi0%2BYgBEs7FeA%2BmBdW3lL7fJWb1gzJcxpu8Jpbw6SpJoQl2lcKYQZf2KxJdnaFElbT2BHUl4s50oOBDMCWgs5I4xHR5BAThcMFr63%2FCafn2YGzsuY89Up4nPGQEDL2oHrMv8iLHgfdBPQSe%2FinRuUL34RDnzD36g%2BC6ZddT0YCTnOx4EXZqmK7SBoWA%2BDE6FrgeNSFOHOkVQCB5jE4Uov03ztv7081WwPDa%2FC1zEjtEu4faNuDM2b3%2BEFtpWHc3twKpoWbzu%2FjVNrHZBwvmqSYDGZRBpoyq7TKi9jNNjXcZYMXkLwB0jqETWY9s93RKuUEXe%2BSHHZrhKMtKaMAnk96%2Bfk8TrcgVMUwwBKFscXRxKi2cVH87qdOt%2BHC7IwbEJObERyHCXGjEhZ4ZMKfzfFk23g4kMYbPHWC74z0cjktorSKH5Zv07ZO83mtMMqq%2F8gGOqUBoWVdYHM3povh3j9sJ77m8%2Bt%2Bb6X6UATD2M%2B5F0UJqfmsjWHqWCuj%2FXcID4fDvIfSkR9YW49Cwbk66sKBnLAcSr296fHbRrPskBMYt2Rfbo4PqV090S6nMQZxF74EgQRVs6ECocRfTwn93Wlt0ck%2BELDqSF12fWTmSjQTuwrk1JrDpi4iXVRVAL5zx62APD5yDBrpvZD5gmhFSKDU%2FH358A06Uy1M&X-Amz-Signature=72b5bfcd32cd757aeeed3d2bca0548b288dc21eeeddb1344dbd97a4c8b87fb6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

