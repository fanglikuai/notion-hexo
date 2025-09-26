---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ECY2HUW%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ2OQaU8izDq9rvbLZP7c4JNOG7eYRgLDO28VgYtz0kAIgTLE17EyIbAJzNEEIf7y%2FznANpGoL8PzKKZGdEy1sSmIqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ6UoxUsqvvoV5b7XyrcA4%2FypfAkjfE2WBnvi%2FQKlzBCHVLmESGi2pbJEvsiBEGZR%2FqWMVx6nOoD7zbL6MDm7VGCztkSQVjvkWPhUnIrgDbwrS8NtJFJz2TyJk%2B%2BWW6S%2Fu1WA%2FOGPgeVbfxUUErS6YeDQUR6Ai5TNtcvPljH6JVnJ8iSqVnX%2FUQ7nRQfW2vv32uHDKIHo%2BewKBPfKwSMaT1ZnV7Wip7XGhTGJV2WsZoPR2lfiQb9BZLQw2S2b69PrS3%2FdWRggeCwjMHKCOeMCOVAa97zIKG0SJ2QFpmrhXFGPyWGqukCWLp9qAUTo1NFElD9OeMITSMZ2cl4VzYlGVhsRNEqcDqM%2B9MzXA0yt6ITeFoj%2Fx5XoNOPqCOb5%2B7qUzunEN97UjHaSq5YOJBSpOOAKNT%2FtdVwvR8Kv67Uq7h3PXVlQbyRi8ie%2F4CK2YOkbA4Dtk6RDj6u0WolZZhLlvurEqlPpxbAwxnhK9wlFTSQkfvo4%2BN94VhoowuLPs%2BMjc0Wq5J6Y%2BeWOpNDWpHGIMNCpuNr%2BlCbge%2FIGyje0WMRs70VcQ7GW9rEEETgM8Tusm7vNx7%2BEIq9%2FUCbIKqONMlQq4Glza2c2hEgvhRNJq8NynHEmy69UnNPGgQkADKdDhRilJc0otnchTXBMLC62MYGOqUBSOKGxsPu9kDDAibp7lw8JEmdo%2FnnusfTl%2BIlxdoEJWK7i%2B0KrpxqPNLhELLFYalA3gBXjqtCe6rmMuS%2BA5LRHxoYS9mhXIGqWoImQR9WIJ6REpFysiSJy82pwSzkMIgezK1wk%2BST7%2FtmUr0B3UEP6rQv%2BuIFBhVf7%2FAvFycHHYrlLH1MrtCuC9vumzjDCOAut%2BuQ7BZ4aSQJ8Ottwu%2BVpGDDsKVp&X-Amz-Signature=87d7e4046a1c1666abad135368a1666ec601a61d2f9e193b490dd46d51c0ee6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

