---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5BUPNMY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T155517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIBSNMuLAV1WXWaSXE9vSdWHJQEvIVJsOdHpW6xgN5ISJAiAfMCOulbIRa26FB%2FTjGGrGfgGEFZpM%2Fzz7uEUIhmYnASqIBAiQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUP3kmBQGida1GHtjKtwDTtu3MAZzV1kZkqKickDfSl29RCFopbkh73FWGZPQExln7Go933fq8Bi9gGtcaX0zaN505nZ8MQFe7wsnbdCUGLeFgWGJymUPhBr7qByHX1238PCz5tmM3PHbXmGOasUikxVo%2BlHdoYgJlFJptyr8gGGItjg97Fz6mpoeBQl8lQmJ29Ta0Ob%2FUAwL%2B%2Fvbiw6j6JbTcD0xhYoDZjiX3SFeaJR1op8WoiOUKmL3IMKIA%2BC8xx4Fg%2BJIm83Oj%2BOENolnNRlfi1%2Bt7BUiiiwhSu4U%2FUqstPHBxzoV8jzaSGMRU22cuZPEdPUMnKwCzGUiE7GuFwoXqlDw9pf8Q1cw90LFl6RI12aecUDU3LQgPVRA60TMihYu2YLJZnAK1iNd4dunRmuRIP37hTbgaWGI2Dy0KJYMRIVGb%2BUfIVA787N6kbN1tGVoqlaWcamoo4aQcyFG6IaCJYAZuXmzXWH8OT1fkcgWbC99iQDNztrL6uE97QNks8qp14DOlE%2FWsy3z0o3%2BOiaypNYvArp6mVwxbala6IuwJ8G4g8eaHpQtHmSpl8ZqVz4r%2FW40d6wGFg04HcLyqvgoSaxcyL2e97Ws4Vuuh%2Bq3kqx4KmAfPoxDFPdf1DdJ72DXYCGbrqoP%2FFEwsuilxgY6pgEn2d7%2Fusx17eJRT7soIC8REVKjRVdnIB2xfieSFxxQR4TdtBJvtMaQWFFqrclsE0Hy8LRxaV40GFd8Fs3Iztw2Et9X6%2Fe%2FIZT3a3b2O%2BFlF1GsJ8DnugTkS1l8SBInd63qD5whBt9cla%2BhCuspv1UZ2Eus2oqtyY6JPmGQnUaZ%2FzF2t6ZlnoS1bJ1MEh6MqlgPDdAhLdtmO7KVoC2XDfwdmFp9YEBT&X-Amz-Signature=4355a714732d7db7c45a6a17f258704b21c5e603bedf3fab226ae19b3a099fc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

