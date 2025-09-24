---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EY5VQ5E%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHbenExfNHSE29yjRNVWme2%2FEcZ7zBspWVMezTq1782fAiEA%2Fzw5GTy6Lo4kFOHVSELvajPYiMIk7tC5PII19Hsh%2BIUq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDB6UIY32gs41bULLZyrcA3oB8%2FMpK09W65LFB8K239lEGyUkV7kkXGz37xQl06PRtZ01NXOmgWXmmg1F3iXLwzdG1SZ82p2IXOIYS3aDqjCvdFhiORtaJZo%2B043whc7ovd8FxVu47PmTLb8NJUiX0k8%2Bfwqlpx9CVgdupsvLVQTmDIcwRChy%2Fe8QLacbxhRAxRdaS5TfMBiuwe1spCKSQOH1jaMpua2mvl8nknQaw9kKrffsgyl1xs%2Bj6LzABdo7QIWIGTKlzt9b%2FQKalYV0Nv1F8rg3biLH13sn9mEP4IotpEGXeXgg%2BSkhn7%2F8gO1q%2FzfDeNnKgXosCtmRylG46wtv%2Fjr9jqC346zkLfJKz25f24TRy6UUb44YLLtjnJHMQC843DSuYM%2F8kznPLbYZSM3leh%2B29%2FHLKFEtTBA7%2F%2FrHyeCt0rlbDU6UYmRSIjzXhWkYq3LDQGNqhlI%2B5gzWYXevzyxculxpv642MK8xDtonK3RZhg0JsMiFNZUdMiYWaOwY5kkEqyYm6cwmAzPZqKOO9VUne7%2BhAJUBH69o55mHTa4mubWFoqK0wkhrr%2BPKqNMrUwXI5iU4kob19IFJy0obmGQyyb7B6RZuyYPozPW%2FEN5fuQIHPZzBAWjmh7v9HviBBsuLRAipksSYMJOvzcYGOqUBN%2BU%2Bru5gp0Z9yEgoqJmXUqm6tsvWL4nCQybNbsFdncd4gkaWzF6l2XoOrw%2FVKRORS5v4We6fRKb4RcsLRxYHVdlGpBnNB57qp4xFhhhWXRsacwyxJBfSggshoKabERLPQiq1Sc2LcBzrSfTQzjbl12tzNLd1VrgYJ4TSt%2BDUSj7NAo3sIKBW3%2FLT1zFxhYqIO9Me8FXcK%2FFB042BGyZLsBqvHnLd&X-Amz-Signature=083f029c0fa8b54dbcd24656fd7c2025d27eb285ab107541873553a435a89eba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

