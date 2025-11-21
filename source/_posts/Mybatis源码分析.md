---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFQGH3Y6%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCkhnhxATd99Kz256e9FfQh8Djc3E6fUi4NIKpE%2B5JgFQIhAMef5be1jrKZb%2BMK%2FZDWh%2FxhX8F3tvNhjsWZRCaVemzCKv8DCAwQABoMNjM3NDIzMTgzODA1IgwiaCB9uDLmtVmhoyYq3APmob1sB6GEbyqY%2BqlaIKtYP2FpEF0%2FE5Q1xl2G4HjIqTOEi5BiudNYE8VERv5lNY4tuh6IpPctjijCeSm%2BPGeCVmadBOdSirciXhNXFQWPdOezrJSLNAc2IlttiCC06HBO5ajk9ggN4FbLJqD9BjS%2FvKdd12KEcw0wL93s2r6soXz6oJsRJV5rHAGQ%2BKrDRwJOna7DuKwu0ccwVVMtyG2u6rmUGC5sZYBQeeMGAL97A2MdU%2BM%2F0GCK2R77TobduYxkoy7ChNecAbu%2BB6OQBJAO%2FNuC4TQWJjz6K%2FSiElrr8ZNScK2T16pPccRqfY5WmPafwUMWZHEmLASXzJDfGl1BoWt218lk2PeeofJDcQ0ldbkWu2ve5uzjyWz%2B%2FQe%2BNoItVJYY0l6MbOAAzl%2FVHK8cEUEkG%2BuIfRRdJScNyTxKX0ukYoaaK8iY1eYApHabvw9bZXAxqNkPGpgM0quLjZYKx4urY3%2F%2F6436TgxGTGs7PQpMBjYgmos8vOyJ3T%2FhqVZQZPAillqiYjx7lDUyFuH2Hew6vPUyslIAphR%2F7eLz6HQ%2F1ikCjnBN%2Btw%2FjTyoDfM0s41CSE0wSQY8GpFi%2FdJ4gByoZm5o4TwaQtJ2deTcy39qrwg95ko4ePBIgzCqloHJBjqkAQ1tz9fACMvB3SPbqb8oEFk3nxNnM9q32MF1PsyE%2B21TZ9GcM%2Bh7AYxX1DLC7OmQ%2Fwl3LDlrGlaJ%2BNvPQjw29xASiPnh50qn4D%2Fhs3Ihq1Qdt0dH0l%2ByUys6GLJKsA4wodavwgiNrcmAVVi0y8Qc%2FlaaYIp0q5s8hv2i5ZTpCoYmTDknkRd7uRd9%2FYfpOH9M%2B5LZpXEI24pD40QUTuP7iNqXqI3t&X-Amz-Signature=0bb917515fc53261e5c0b828258ac3ef46087a9949ac9de20f1a5858a0f33601&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

