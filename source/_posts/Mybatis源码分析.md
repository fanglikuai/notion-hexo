---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STEDQ2QK%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T200052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQDG%2BteVO%2BpmRqzG0p61YGgEcLKXSF%2FQPtq8GXAU6bykdwIgWMzHWc%2BRWBlnJsh8omOQBbOLRj0SuoCrVEHxr4C%2Bmqsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDK1IABTxFE5FONMApyrcA4FcRRZ0kdeFD6kbYXH2EnNZo2J7MpYm7ZC%2FBv0KxqPP3L3fr04t8idtJUk7Cn98vaswXzZp1Dhsy1nAOM2iCG1VlejVMwE2d3otRg5%2Bf68GeeUvjoXBnHe6mw6i3w%2BfRc8gBCaxyrjI7TwKJHkLQ9TOTMXxYLSMnsfmAk3mbFeh19UCYNCY6uGHXAk%2FIB9IjJO936PkqAm2HV38Imw6hdTXFOYA2bfUBZ1%2FGLRC77z4xaVcxVls%2BIsZyCARfs4E0GeHuu1XycyggB8ot%2FmNHm9W9DwdziI4q%2BdEu5fvY2Dg53PUsjbfvy%2BvYXSGYeWWpW2R4fsLEk9oloS52r5Akt7Cy%2Bh4BPSeHCB98hvfEJCoqucBSiQAJV4uihoCVvz5tjNhKYanTGIRigq3cUsb6owbjzmVaeh%2FwAdJWaJyJtiMayEE0aty2mDfIcEE3HHY6qWiAQ4okppuExt%2F8VMOU8NRkBshy3efhhM%2FuFbSyWV%2BrAe523TZuhRu4kcZJNXfR%2BPj4wI5RkEtiF62RLq2oCaEX%2Br1RpiJlcNzh6yvLZmuBRmlgG%2Fn%2FqFq7a%2BlOtrYIz5uI%2BW0zgCAhfvZsl7mCqbYQgEJr1Na5uTJ9GbWMFyZJco2jgzYuCn%2F5jW0MI%2Bz38cGOqUB3TKfP4Uu2pFuJiI32i6UEtQEyouTleIO1H5SSYJG9HqlnxjaGGSmaXFKqx9RF%2FcouQLSD9%2BcKDYG2SsestpusfGKPouE%2FBeFtHYyq3Uw3Ht8T5ZQ22sY6v5alPOJ9T1LjEdoxnCIf%2FSLP7h7ASR35W2P3p5nRljqPpkFfhnkdbfdjtc1Ua1VzGwcooIC1CahWvHFjLicLE6dTrJkGmbUvdI49sY2&X-Amz-Signature=9f2b6c724ced825fd908e30334854c0d801439bf305df5f0ecfd4cc40c59ddb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

