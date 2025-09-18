---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWCIA2EP%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIDGWL5aTONU91RJHWlwBdUraf1JKY20HhTDIyQpgwwVfAiEAvgjPkm4T2OBiWNWqDYJWusgYNu2B5zZbMMUbWwykC60qiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOugUfp90UJENbjLCrcA7SsIQlpJDsUuL68zqOzBbC1LVZo4j%2BG4s437PAJesqc1sz1rfq5ed9SipyIvmh6U13uBeR4tgwukDmWtMyjSWfafujp%2BKw9MBsXiba6%2BbSXEOIPrr%2BJejAUetvOjgN%2Bu8ij0EctzRd%2FQEgkDu2L%2BnConr%2Bj8haKdcImJBgY4k6u6q5%2BBFNjl9ebRhupkFyB00w2IpC%2BH%2BrjETuWcE2SjRcllWqXmMTMWIJNhBYcFXsf34Yk6dLrRY73nKKd8Tyc1p%2B5VtUvhYCektxO0aVgWH2akTb%2FaftCP1EywabIB6y6y7E%2FHBzqcvU%2BOuaf0S%2BBXzuzmK49IitCJ48DJIM%2BTAR1vNkGc7GfrviDULsTOhFiIN2kaZEAJD%2BODejljZBb9h2MglDY%2FYNvfEdY5gLryjrIgUThqqbUSlL0juHDBUajbjbt5%2FTmOU3oA2CB4PfYz2SwQV1TNTuZbx2vJFByFZJn%2FTig1LI%2BhEw8FoM%2FdljgDDmAmbWuVpUtmuCJodY5fArMvKD32%2FpG2SxtGwkbdEYSQX1ZgW5ZBRi%2FTmOPlzvmduE4ndjn8Gh4JTDo9lRKRG%2FdHOf%2B8Pd%2BS4S8epvE%2BoJF29U%2BKLS21rO6n%2FwQh%2B73utXsvUUldX8QeOflMKbfr8YGOqUB99av0G4rfhPh6bAjCoPCwKSIcZuk7TODu%2F8wvxYdArrdeT6DFLhVcoe9e7WsXsSEkPIQfMzwIb%2FwiWLI71iF95cHonbvI%2FWpe5y9JkJEqYZsWPEP6MZ5SJs3RFQL0PCT%2BpjAR7gd%2FTZVmHnarY5TZJzl3vlOQyRWj8zMjUvXOuawh8uC75jgbwLCYvzxbu93z1eBjYtOSKZDQ4zJ1ZObVP96O%2FLY&X-Amz-Signature=4df19186b70e9a86ff30fc20525b18daaaf9ff388443da38e06a4c84cc80c466&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

