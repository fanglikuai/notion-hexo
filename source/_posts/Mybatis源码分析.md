---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLCDBYK6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCICIID%2BVv74%2FArAptGoDjzupJuA%2B5k5n2PDDn5F1X4Va0AiBMDtZDDyhYXPNd66%2FyowDsWrGcpkA8p%2FMPHmMH7GhVJCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHtaGREQuekLPM6Q%2BKtwDrQD%2FU5q4sQghkeqEv%2FuN7HjBZzzEw5E%2FtYCJTkcWShOnzVdeZL4D6kiugBmWhfmOuwVYwDNktIiQ5Wo%2F6tjUBX3BzrGrVVJHcR%2F0255DTv3i1jt00IYuNtFrZwDHIltygDVMynJ0g5ZkEgv1m5qzHv59HEMGvIvQ%2Fz8Wt4tD%2BAGEeA%2BiedZtYfy%2BsSK%2BJm6V9CgheTUOoGkoKzc7TD0t5Uv9OE%2F7Etqi98xNl2FtmlMgBZs9FqmfKw%2BpB2E7NSOza1wxHG82owz5ifeunQ%2BOsY6HWPVT1NuAFFmj5TTac17UyvOemJMHIyUnYGKVPlUnXOVZ4iMIfkGZuVwOFIPsBpHT6J52MkhogLB2ykK%2FjuA4wn3hzsX%2B%2BGAqnkvpOiLbrL0H%2FXoE2mnPGmj1OSKwXAM7Faod4y%2Bf8M%2BO%2BPPJHU0z%2FgQoQ3RrjL9x5URnblTcuWmhKGyvDyU4X5zUgMcWjZDsCCMl3l4NblAi9AeEWlhhDaNbmss%2BcMtB%2FouDe4lLF0ufR7YCEnQZeUjsypu%2Bsn4fhevME84sIjXGupLYd36c1FMnP%2FeFlob%2FCQMJcnIDmZ7sztD6gIn3Bj3iE7UwIo6MA%2FTa%2Fn8T8yYiKBD0x9wypE0P%2FGqVxulleW8w8cKdxwY6pgE%2FxE%2B5hBY607kZ1Xr2ogJogLFdTA8Al5BZNehE6D5lVgl0H2mPvE6kFLN4WWRgJFU8MfcGIDqpnrjx7P6vpCUkNo0jXmXpa18YYVcIqwXzSqQyPc2cV9K%2FaYUcC92dO64JR58HT3fV2zK7wcPiTK03MwnSEkMOsXH3C68iJWzJ%2F01V%2F0YrkTkgh9DSmzO56AE8ukRivCVjMbo%2BZ3237EQ%2Fi8aT29Vw&X-Amz-Signature=34cd324853794beae51d45dd88adbb46fd66dd3eef3ac9479d38e759f99aa444&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

