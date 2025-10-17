---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676U2FF4P%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG0ZHP6C0%2Fm2ljowRekoozTN5O450gMwkf8X3c9afB5qAiB9%2BaZHSvGyCsvtnLNeJqpWVp6J1Zb%2Fb4eHTuCjAvu0WSqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSGzxBVIPv8rYxH5tKtwDrXJMBYBRFZbOO2BezbhDwd6QUPIMIyruXm5uR%2BsDSqr%2FQ5n%2B6mfZ%2F%2FsCHZU7skTBSArnDiTPt4twP3yRv1Ut4YJgHeR%2BkbRgSD%2B9uA3dCWKGDTUi%2B06ou%2FckxqywGg7UjeQhBMr8%2FpKH9JtDlIary43m3F6PAgl%2FjPkxAJNU8PDDzP20gL7YdYZvhEzDxcTUCzZHWVqDqMr23X5kElcJct%2FGTA7PMpTXukvCFN0ny2qEGieAs3DkxFrSO%2Bqtg50gPFXuB0qF9wfK45bdaOpEOMrOlAjB%2BWqy1Y7G5PBrY5SWKAvhHVMC33l2FdRdEVRKFiEprc8WvUr6A9KqkV0476ZSsT5A%2FS40iH1q2SygxAyAGg7CEV065JYkDfb2ehKboLZvZPNqLrTGXZ2da2hxPkWucvRya8IEe6ibr44mR3S4I%2BoTKHCjwfupuf45E%2FoAibJN1TCS9Q0yn01iFTgewvDast7xt3TlfIg4viyinER4TKaZZiDiHOSn4L42hF%2F2MJf7G7GiFLq4DF6el7pEIAP8Nc3NxX9%2BT%2FaoloccmGDcCPsifqei7VTSbip6AgV1b7%2FhBfQnLEmt00XLeGSM3iMgEDFlxBreMTnGt1lzH3azSCG1gSEtQJbY9OowxejHxwY6pgFZm2Ocev2bBL3I%2BrZuOaYj2yGNKjEuuN1TR08rUyeG8mR2%2FbTEhE9i%2FnzOlfiCkRFWhtD%2FTObmp2NXa5whGsaiWUbiNGN7PidWiLL71BCEdpU6EIisD%2Fxqb11dQf6ZbNj87A5OFfce85qAg2FkmXoAwHds3DgFia%2Bsu8mVcnEMuQ40q7yUOhp3Uhr%2FnSIZh4gfFaTk0tVCoeh8FvMdApvNWX73lNVx&X-Amz-Signature=86b72960ed61d5c1da31f8ccf428ff9fea1e2244a9dd50074b54f45a112a29c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

