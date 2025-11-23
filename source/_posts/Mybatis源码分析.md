---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRIRXAZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIHbTkhMlj21EjTIJgd%2BUWbIuGCvakg41P47a%2BTlDh0S0AiEA%2FLvTRKrARyRS5RTHPvDGkC4%2F%2BT7TL7WdilIYu49iI0Uq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDLGz1NV0WtFRWnSAPCrcA07eh3%2BivmO8FFJr5ZFLPNdjbnjApypf9ckUSNAIwi7NPRsJiEmLPfku7bMUZXviYZNLNUmcX5cIZDWfpnOFgoLa%2BGK%2BmGyPGJFa1nR4pKGzJ%2Bn4LYulLvmtIs819zu0Zl0pCIyLBDyV%2FpFPhf0SjWpGcZbP7f4dkE5yw4HvwzHl3wf1LWTm5kCsamh2wYTRf9pH70QLhgxqL%2B8RKia1z3F5SGyr3JcIG1upva0VNQKxg1ysiDJ7UP%2FPYgUnG9srl6aYWXKzzX48t15qfoUMKOwhC6uZtHK837FkVzwaMJ0Qf0YPz8CRhD0U%2Bf4DlQlRVcTApn18SLQKLlu6RmozGm6Qe5dPMFJMTqpnkh4dP3qp%2B8PrRnbai1PPxVeRtOgPmPGVJMtUrxNEKosnibNih3CUKhqyvj3lNrw9la3r%2BRnej%2BneyFqlYWGGeJbxs%2FE5gOIy2OFdTUXM4PhmaILWmsKFLSLXHM2%2FDFUuIfp5UA%2BKnOQBb8BmTikbNTQ67GJIlcPMFH8yGG476xaEyu%2BYnOQiXHVTWU%2BB50zOVsSouL2m3xHM%2BN%2BejS0W6ERktoQfgGaj1k%2FLIcsBBTkavPS%2B6rZU0MzhLL73S8v20JI2kPvS7fCV0G0hJDC1hinwMIO7jMkGOqUB1fFW6LJkFvEi4KlM7xqihspeiRFiBVKO2jm5sCvMKCy1TdvbK6m00UxgBNtICswUR%2B9QJRfT1Cet2SOz23zHx8HIG8urQJWwM9fhoFJxm7pMnbE8ImODiosbPTZ6Im4M2nvmgeuuYwYn%2FbjA23dND2%2Bc%2BrOaQFZHn1htZmCQ1zepcSSfxcLmby5tW4SuLpSief8SZCSnQ4Xb4fgD%2F0n7QBBTAqFQ&X-Amz-Signature=0119df508b1a045f87da3ad7c14580f6d8f3ceab9c18649021cf6f7f4dca7519&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

