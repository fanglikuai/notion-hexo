---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYKBOBOZ%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIBgGCpqHdeKPKgg1zlkrch%2Fw1v7uGfFW%2FpNdXgZwTavBAiEA9hO7V%2BuCqeIIvu0yxNUCV%2FGaNKnAH5UXSPf2XEwXowcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDBvJaCiBauDMRSwgzircA5uvgNxMJzHiQwqm2K5PVIPdGGV4%2FQ87FrUm3FBpCJtDoclGHFxPGm2L4LEtiUp9%2Fic9Fp%2F020jL98Arz%2FC1t1tEVEMci%2F2JLSRVUXzKXG3XoS%2BWM7pK7ZDb4%2BrGljxR02W%2Ffq1T5AMf5kjFNLH%2BIFsPQugHOglcNHbkPyYTSKPrCi3GkIuAdurj%2FQf%2Fv%2BcRyQzaoLPSF07V9fxlrQNX5KjPe8F1oPc%2BtFz8JrbYqhhT4uhgR4qdUfX73Vv7FuzLO1XS0laVfEF7gpmnhRdTDR3rNFwfCRkVDB4TjkSdHvNC8xoIsMgfquyIgx%2FZtebjivixdVvEaAQkl2iCR%2FPkhE6nkJE1aQfZGOOEkHdQNuwSH4vkJNC1Vf%2BToPpegm4bU%2Bj4CPYD%2BJFMjYl5M4hPqHpFFerPUT097zcgv%2FIBz682s0G%2FMMvPfpVbNMWCOVROqp9pVyZBTc4%2B%2BSUjhNU00dka0AMWZNeRkJMKWfj0a4od8MSrTbBfdQbVgErYEcoTAJcHG1UWBT4MgSayvV30GJAdadaoPZ%2FzC%2FY%2BR85KnWvS6uRBCEQ%2Br0%2BzeDPCMVOL8%2BHTMrtvWuN4cRPEx557NpKYD1%2FiFW07kNSZZF3cZ8SI6nGSEvc3Z1XE9Ss9MNLCmcgGOqUBRaB%2Bn8d4tYHiz137PWUz%2BkCFf4Fdrg2RBX2q1YJMyYMT2NX0bRasoCedTC%2Ff00f9N4svGu24ee%2BiozFn7mAKY1X5uIjIO5mA2m59TcdaxiiIxdR2MlzrPm29uwB0rinRISpooaePseh1sUU5GVvjp5hBP9Jg2BY3lINL4ETv8J6eviz3f7FQiYUdjM1Urm5DCGSdz0NBcxNOQUrl5erKH9UXkRYF&X-Amz-Signature=8739b3cf78545570cd0a1430bba0237539aa02893f8d2de3aa3b9b07677ee755&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

