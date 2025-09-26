---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PJG54GD%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCICRhW0pUMjQNS4Klkm7PxFfNE1SKG60yW1upk9fuUgL%2BAiEA3AD4oPc%2FpdhOfWrHA5xgO1SA%2BDfOQ5KMGIuhAGN%2B7VAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEj00aWn70Q2fESv0SrcA94A4nyFub0EQSXl7Genarrhctr%2BThUCiecTg5ztznfPR6a5pqPB67dIEcd7ncoPRYCyOBJV2YNyC1J5CVUSPd1If2B4XqN%2FCoaRLDTe%2BZUMV7%2B9SRB6V10Y0ZDr7aRP1GYEdLCOjg2iFlVUYrTWM4LbeWqJTb6KeaYVTOzfQ8h8yXBgkt1y%2Ba8OZ02xCSyeSyfuWuFUP%2FbvT7TFJnN%2BrD2gyRg7LcwPgjlV7RzFeODsFcyo%2FGOMUBeOCR09hM8ReM55hKTThfGYVApjJLAB55sCbjrYi2M1o7AOOlpcykmmIuM3%2FxdAFWAZYgjESUaB4ErnQ%2FZHn7wYfaH6ZMSCH5RHtFQLU6eL1JWC1lJXJdy6Uo8u%2BmXV7W0e%2FCTnKxXdEiHzq6kMijS5GRVMS%2B2E61StuGnq7Seyum4DZqI0L0NwkuvSzUWivq2G5h2H1TgJTtdNTClr%2BYkf0IDPRhY6spi4FM06sU3D2u%2FqaiH6jc7bMHbGzkSRuY3ryeO45GWOz7qrBVgcHpVATDOWe3TI%2F6NHeoqSQ6HAUfmTgRFeK5L8H5oEg2FDKe%2FBxg1paRZCK0INXVns5ZXFtr1tZUtmndKMhLgRacH5lR3d%2FLTSrTIy2YLHtjZdqpz7tJmpMNfK2sYGOqUBNK38f7mSPkBCmT%2FIlm41bFxBUeyygy7jd8WtLBpEaNvZz2Ojb52SFypPhdGh4w9qBJVESk8eN3l4sTqkoPLPu4lc7vVznuqJXQTRZUhnGMUB%2FrAKU0EQYL3hzo%2B7FwHuvBpz8BYAFkkZWPpF%2BTSQD%2FCWi%2Fz4%2BFJ%2B9wc88V%2BvXIdXJgrfRyNJoRcPuhJ5ULMmm6CnzhxYzWbU%2BVBN7Ug7wpNkzsNo&X-Amz-Signature=37e5758bc058a337d1d17f010a7a158fc1fc22f64258b7bcb89cb72336e75a92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

