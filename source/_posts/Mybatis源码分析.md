---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U4GCPUB%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaOQmvEyFFgeX2U7xLDsOqgBWRLdL9xEFXbNYUxOnVsgIgdLktrfJNk1hakTDuI%2F1ruj8v3cdOZ4dcIb7wd6aj4Hkq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDCl9o%2BUQl%2BXRVy3pKSrcAwLE%2Fw%2BU61jTQ61EPSPpj%2BUy4tbAc%2B4Mfnbc7DewQqWg9b08jYTk99xki4Slx1mQ1ojX2TPoILuosezGXNCqZVAVcvxuheTjWAoHm3yB4GjVk1EJL1OnH028TDBpjIxyQaGvoQriGQXiZz2vicZRW6OD6pKQEKU3SHBIF%2B4AC8Jov8mM96alV4eg4SkAFygtcwlzliL1RCw8ktBcZK6cwUu4LZPORi9f9PSEr0lMkP00LMCt25qExaE71F%2FfqTc87vLVNYj4hslqQ6OKPS1uy6tDQf6mSwkYMKR8mMroV6x%2BoJdYzZ01kORFRY33rrnjo%2FTXcyMCjq94cOPEQFLUXnAGKvGkwSu0Ax9ImSlA80dc9FMDGggM%2F6%2BSBCvO9%2Fcv8y%2FvOryfKHOlRVDQofW6HGj9%2FN4o737FHvDXSfzhTdSieUQdiWmrLjcDBGm%2B9z%2BywDR%2FiGoO19r1x2vrDcFZG3%2FCLK99HgDdDhgkjRb2t5H6mI9cWJkLgjDcNbANi%2BnCqXU78DIEPdK9NIb9WpqM9JeajTxW8hTMZl4%2FspjCp5g%2FR78aSLXTKHWYU9TD6k5eaCXgknEMmIln0mzUTNzFxJlOMA%2BEg4LEH3UCA%2B3m76N9RKITARSpx8WzDYq8MLfq8ccGOqUBlGVvXhC2%2FgOxwJFZkKXMVM2osj1943HiTC%2BJ29DVJkIsGAUZhA1Fz9TRSGuhXuwhXDUAvI840aqfMaOEyi1AQu%2F8ymvKzX7%2BrwMGQKpR%2BvapCYl8aSeKzDjd1cIb5sKCUCf8iQ3lbmHXjim5FQwDoo7XFCoy%2FQdC2YZLUHKwy0K1NBenH960Eejm57Ux6ZPA5gd%2FfftDnklLDBLtMLgNUtguTA4D&X-Amz-Signature=d19e35ba39c6e925e5dada13d36de9b5ecc9ec2bb4c9b8027e5ddac67128976d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

