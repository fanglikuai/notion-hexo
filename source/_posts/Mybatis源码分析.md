---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7CKZCJU%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3HB3JJgpWQCAwShfAFJ885540iUovHJljdCmlkq%2FgBQIgHBozo7mqKayytT0J7Ab63UBnFIBOiC03For8qMv0WigqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKdQ70ncmN1ZUet24ircA9o4b%2BL09C2FnGpVOVuMm8x5QxQfJjMz5I%2Bk4uUKed8XIGR%2Fsj%2FTO6jg%2BKn2Rtqnr06%2FFhYLcqBEXD4tFYsuJTJMIVeiXDaBEFsJyWoLXSe2alSVCpMoiMlGT%2FC%2FBYc7%2FQ4R33OutA%2Fzl53CemPkVSVrzJnVK%2FYZYmBKsNG85C1d4Pdpg%2F89VFoi60IZw81MbBer8ROqXXELRhNR4IP93%2F1HlC08m3HEY88V28keajcvxKAIMqYWb8ckET5TT2ukoxHKHZoVpDbDPd7VFhNxS6TcykAmcqFknFPV6PE%2B7JoPHV%2FMGtqFGeXHqP6LpoI7qXSVj0i%2FUqlr7yVY8sLsHYeYCxlh9PjwU3o%2FeU3pU1%2FuESJYWOVE04wwRtdccjhnvTimnDAxO5a%2FsQKAutbsBLlA%2FKHKdRVQmKLoe9GMcWs9DJq3dnbAKqOM%2BxMfN6YNT4iX16YyEplerQLtVe68GcOvSaF8Ii%2FsSdQtBn4FHWOJP%2FQSidtaBe3wg9ykotTd5Bq01%2BbjITyD6ZmySjXAPSUD5s8RBN6gkKkjm8o%2FEeSDVYfuKLk8uwb%2BVn2NINDivofMRrsbJ08ma3srYcBOp679oc%2FpHw9O1hgyXkPChgjNzPME02S2IimCFEzQMIT0%2FscGOqUB65J4x50qkv8K5RnycMQ1Pqv6X8oak5T5c1hbyl7o13xrcQGdihc1O8dBnzNjYwwYWZ7yPQmgalFq7ucawsfvONBmDpurKn4XS%2FW9vZ3%2BwqshHpMVn0wVkmzSXULQJ5kUCFbhriACet88drqYfNriA5uSWX2txynDp7SDEC05OyUWaztYlm0cLomlM146vEnwhHKOipXDAMYfkYvvpHdNnswoJy3a&X-Amz-Signature=5463b9982bfc849db1465e0a49e2a1ab77b484ddf240900074b50c460741f66d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

