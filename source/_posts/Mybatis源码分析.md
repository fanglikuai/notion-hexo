---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ED5NKWY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCJxx1quKcTDqt9YoDjyQ4UOwwMVSZvMkiJmsn2TfGNwgIhAOVJJOQ3tmKSlCd3Kvomq5GUKWNdckzZxoFDM2QX7ietKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyxk1gpG7%2FbvhNJ99Uq3ANw9pPKwjrb4PSnnLdg20ScPv5Pk16ro14msSfSCVsbRph3EfOeiHyBINONCYMu%2FvJSBrhtwUJXDD%2B5eBxQrMPquWVTLVOHmxwQb7EeMB2%2B1e15LwWYVYk9oTobp15%2B5LB0fSE1zSXL%2FJo3Vxs%2FwsxTBdZ50dK2QA8OlEcdXLy110WjVN3t12RWVCGsdQgedt0VyDGC4HAm54dCgHux4r4HrgDh0iRdKM%2FRhnSkhq0xVNQGLBDCvnX5zDYBaRDLkxs6v7J35FHvHl3b60bz5RAut1u5WL9rOCKaT2%2BD8riAMjZYDZ3AcOg%2Behmv6aPoRnd7fXp0IJNJObVwKgKwLgfYB4RQYHGdTHkHhamqkaq9zipcBFC0ZlK0QpG%2FYfg%2BEM%2BlxSE7jPlelZfW10Mz9hQ3SC8LkoP%2FuFPB3Ho5joKMueWXyrXMncG2MKq8ABjlqmbYdVbYtdwsIIP3yiMYDGgArKHlMtKJjrvcTVaggkURdTLDyohB8SnpkCqkTHMQinVBZR9DaWe%2F9wZErLaNerEGuiO8%2B0R3pDkpzN3clga5Y3hoyJIM4lwIiA9lMnrmkw6zFF%2FCWhWS1mANvFsAWXqRBPxSnj2YPGGh8L75JtdARCP6lkbrS3L7Jlht1zCd15%2FJBjqkAdDeEpMT93JaPszEiG%2BKNki92K4%2B5nT716kACxLGaiYT3tFb0Zr0edQmWWT7dbkt2lSm0F9UCUA%2BnGJsgI%2BHUrt1B32Z9RJQ1qVWQeRiD3nPtBfKlJdT6Wf4EdqVDP92F3ZOFkWW45Luyl01ewyFSaQa2X6o%2F5ZLNbcSeaIXmrIbMymWnP8hfDXxIs6E4h%2B6kFW8%2Ffo3VNEUNc2oobdSbE4Vj2Rr&X-Amz-Signature=041f2daf6f8d3e0d6731284a169b39a077976fcf116c8b870e6599e115d36a5a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

