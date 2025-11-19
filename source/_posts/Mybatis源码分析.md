---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OPBI6DU%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T060037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJHMEUCIQDdkqyVfBgMo8pVaPErocfCUCnppKOPrOcdjQr6p0sHMgIgCciwBCvSAqgoSJZJsp44lRVbQrt2%2FrmXNcuZadOGNSgqiAQI1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGAR89Cmi5zO9QunmyrcA2n9G8uyVj6h8HYCHL74Mnny7zH8eNZs5B6l60WvW1aoQFpa4VuhiYu5mfAOZwH%2B%2FrPQM9oV7EwgZQcLQGqpRdRE1NEofPE14iJbWyuBLkCC%2BUO2tEZz83Yd%2FYjjprib%2BhxeY%2BRyZjZyv230bnPBq8C8H%2FuWXqwU5k8KPfs1S5MuZ4X0CR3ppXOMwuMxEO7GaoMrYJlaFX7BPf%2BNvW4j%2BsrY5tlKSryMqfXkxZzn4shtpAR4XavMhpgNzIzA9HSASZTzN%2BXP0CUPfclv1upFB2HaBb33OjwWXuOEn7kqWms9kD6HNtR%2FtFuyMi0FhsJuWco0KyQL%2BBk5uJQSV7BVzR92jmtQfpfscoD1nmxZ3DrEqEjb4PaxXB0KdkLQac3WMnkUa4DJ8R%2BEYQF0OM9WSnBUsfA7NX2ewSewSmognrFuIbJefFwVEd2Ccrcj832nrngqwuF2rhk9PBWmyl2P0fdFaFPavSAum9J32B3aPwXJmP48oSSwWI4TcMmfNwqJxCVgUo1EX8GfocTEtOqUSCb4W9ylHS2oLInrxEjI1LMOI3YAxMLCQ0vimQVcFX6C0%2BQRTqPECGe9Euf1PkvUX%2BcyBXtj3WnQHQUjILLO3hsmzaMFmPLhSnFNarxcMJGz9cgGOqUBRsBNwp8IqHz0hipGqavWcWKleqol6rFXzUQf95%2B%2FjOgOFvGgXTE7oyozPrQK3jkB%2FQ6KSLkAYYigFwwW2Y%2B%2BvE22EvuwbEo6dXwSXHgcQI7AUawIvjTcq2fYHPNsSdY9jSJ6sFmeyZo4yj8clw4zz7X1TPTKDO203KAUKnWR5ZeQjTuf3HmYZJBAQ3r0SAEvibVZ8U8x8ZQyl60Sh05NGYcUUhqV&X-Amz-Signature=8a57ec76ecaf80744021e1c8019eea1ee0e4449d5cc72acb3997547c60d2fae9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

