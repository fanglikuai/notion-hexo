---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDUVIP4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID7bhHLPeh4W8XO02GXuhhlkHihxISYhblblvjNT1mKDAiAWz8byUFajbe3fp5szNOYzkaQr1pIDjDNOEyTvQKbvlSqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqwf3E5qGjyjBr6m2KtwD1LkDUmXGraNZF4uBbxV4w9z0nVXd8W4BoaJ4ZRXGVMDMy7P6S90EEMea8XaQ%2FA2LVDaFc5qXJi6ovMD5YSFPLFxUejD3y%2FlYqgjdbH8SFRyc6Rz6SwfCWqzzw35owSeCRUT6s%2FiTzdRxCXob9egvKPZGhkSaagGd5RDfYafUo4xxM3GBJzW7cVj61fO8%2BxDRhDdWa%2B541OfENiIKdt7vbtkhJ%2F3gDBC%2FkTrOCOa0q5VueLCtBPtLuT571%2FaNg3Bq2GDFSCDtcmRLcuhceFjpWC%2B2w%2BQEkQJpCxcuBZ1r19mh2%2BNx5xpsOydXUzbHwP54H3OeeNQSqL%2F6U1C9mPFrHKL6bxz4DHZ1FE9cBKIWwiGOoy%2BRwBmD%2FXI4Wn%2FHavwXXgg7abEd0B455%2F97M2k6Re2HlY19N9vlkP2oYx66rBrhBFFnK7xPnQZVBmi9ANee09SymCSGy28R2xae02UPPUpFxqeBKFktJwDvWtyiXiC8K5syCl3nsE%2FQD2Chmn52Fr13G1emYs3cyMkvKFermA7HCeIM0O9QajB2T8y2K%2Fbb6%2F2IuC805KGvixP5hj5CkcKR%2FyeMnUlbvfJiHfd%2Fd%2FLvPcz418VBwERCyUawON6pEPjBtQP7tYBPyngw2dnCxwY6pgFyCpDF%2FNURB%2BS3Ogqude%2BUgE6eMMn0pCfSPY2mXyP7ISW3RpWF9evMuPtmIwzRbkbsB7J4RiIrvJH0yKsvkLDwv4nyiKP9TVbp23eDPkDBc8lG9KptZYqyAPq%2BnP%2F%2BTFMy4tYn%2FhAxQW0aBufGXH5TMzRfyyFdnOS6w3d59cfKHN1JDz3Fa4tts0rcKNgNTt022M9jg%2FoTCfeNKKS%2BhlZVW31zRaW5&X-Amz-Signature=af485d05e69022f3e7adbbddb2248e810e1406092d83e99d077346aaf2b03365&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

