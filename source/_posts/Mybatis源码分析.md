---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMMED5KY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIEwAn%2FMvuSNQzfovrFB%2BzzzWhyNPMsq1fsLM6YiCdY5%2BAiEAqhQfzKGFcRdC1CQ2h3M6h1QmWSHBPlpdlqFf1Z1TbtwqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQ7LSuKREgcaDPpMSrcA0Xffcppaat9xQryPiccR4RlspV2n7UDGcRbqjS1pC%2BXAFC8BWjLU7TVtKVPsyAw7VsEkb3AKxzUdZrEKuVNd6VKQL%2B80oRnjrBQ4vyLbjOCQ1RHnIArysJkI%2BSG9S3kL8oOD9aScTdZENM6kDvHfSKRZ87VYPvYngEf%2FBUinkOLpiwmeKhPB5EslnrkZudK%2BP5Q2wzEpS8FTtUw4EC7OcKACpfgFUS4ji2obsTy8P9jLo1EUCUr0apifg2N2hKEZKFz2t4ScpMF8zpaMtNs8Ra%2Bqk8EIKUA1LDmndgbg%2BERDhlFWrHTNvZiW915qDk0kbVq2twvaRWHmfR0D67ikx2GcWOWeRQYR0xuuVa%2Bx1puD%2FqlY5csaoooqQWK58o7d1I7jjw8mQhMNormaGBizbpeFRvSPJPRJD8ZRMhipnp%2BCw5LQDRot6l080Yf1xlJcEx4ZE%2F4YsRQcGyRdi2TY0Fqq6obj%2BghplKjuLMpf29EtIorBdnN93VbQctjAsEI4529up%2Fuq9JC0SmKUVM0YAzWE%2Fg8mdhMqe5IC1oKeyCQibj71TzqEECAfbJSG3KFj%2F4EWTt0rE7eK1Kv1okwexQ%2FWAgP%2BaKswxe%2FVEE3N4NTLvtOqOPAYY44lyKZMKKX8MYGOqUBiXhIVoNTstAZUGmD37V64lVasAUOdQ2rzPLhkw3039Zw6L9shicgjBwLbqleAKqtZBYBLWzXNQXk2n8ad6FVgEjiC9kvM2MYzO83t%2BtBC8BI%2FrngSEdHfbsTDRkMPJDAZ1RROkcCe8PTd%2FKR9gTofJ9kR2bqWrNkvk4neavp8tffzjh0ZlIUK2Rs15qoT1uS0c36lHiVzEAa0nWYmPi95W9C0hKB&X-Amz-Signature=2cc878e49c9b24bf99bcd772da00ac81a75a69cad97377070c40fca7d6905029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

