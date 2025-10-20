---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XNMCOI%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQCt6%2BBA76M%2FymIKu0jH0LTSkai6m33vieNuu9MUMI1mQAIhAPainWBSg3hX9ompSCdWKRKsnFvRd%2BFARBzhnSJsglOHKogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzGDUi%2BQhMN0JbPFV4q3ANrjyq95t2cnbX64OoD21GKAwkgayLJK2IVuoDDor3%2FuwISwmaSM7LtLWUoyb3kKFeJzH3CIRNOBi6EsKy3jU5A9Nn3cqhX1BQwbuRHmuQqws7PbQFUYH%2BmcqOfYj4Q9w54AWoFh%2BQH88I8FpMmMvbSfng8tdnlRMHpjx4nQe6Rm55fpigVki3xvra%2FalqpJbHFBghhWqa4qVKYSPIFUjWDyoqpptfVJcbA2Gt2ws6vACrvpE3umXWZikw%2BoGSlDMy2sA6UFXXGFmCsfN8blNdfGVG1DWsKCFWp2FSIQ6X9ileM%2B6DL5iUoUGxKEX%2B9%2FSTGtlQXkV2Qa9YUnVMgiDYYoFVapfZYN%2Bwb9gUdFXCeT%2BPIRfHprEV3%2Bwg6lyYFjYTSkRmqNKo%2BL99siOO8cSj4oVT%2BJ6u7UsXyJTUmqGPxpSm0MODrf75lM0gNuca4sHCfM5EKzCGErneqQ6KWDycdm9v1tHwHfSUw%2FAdcGqjAw%2Fg1eRIlhRi3g83TnvxhOhDTs5Gp49rWCiuC5i0hlY3Y8mfQC%2Bq3NGp5%2FMLc%2FSJ36ckdBL0UtcPJa3QYs3kmYRSq2sCUsUxYBY6qPwo2GpCLl1Q1KLoZ%2B5yBeFzgt9%2B%2FycPZazFO3m5kfzR9fzCg99XHBjqkAYHBwEDDRRVl3BujkkGA2VzWSWv6eoHxRtz1dz%2FPsOmhcXvU3RKJbZaErfqjO%2F%2BCECAliuMsSxvD3eSfPkP15%2FwZBSVQRJVDAbGTOq%2Bfqi3CWEkDePV4TuX8XPbtPn3iy4uKxktuomXTClD9Gtsw6wHPtKI4VANHvc3Qm0BqP0iwf%2BZ%2BHILR6ZInKZeDl89fqDE5rdYXNAcCOlUkz5x3lnLbXHqM&X-Amz-Signature=4fa3ab47a8c879cbc91d13fbecf35cd562f1716aa3e9ea23d712e1dd62859e76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

