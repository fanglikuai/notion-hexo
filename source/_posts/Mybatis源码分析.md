---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633NQSE4D%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAc1SXEt52kyj0%2BZS%2BX0xXh%2Fp6EZzQQur4Zdnfqmjfg3AiBPOIFTzbU7sxWMhOoHaIm3jS62LyVXEo67ODGroLYLBir%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIMeIRzVUrjAp3lyNERKtwDfMrOx9cZVgoBjg7h7dCmia9GIhvHzF5xv%2FSZouI1uml8OrnMfGmY7PnBAUZDry%2B9ioo4n7oX%2F5L%2Fe0PaZaFW0IGx4sJwa%2FZwrvvyK%2F9pJDGMEMcqV24nmAg9WtEqkc6vb%2FLoleAQRS8fTlxsdsdfV%2Bkz1PicrdsIP4JXEW%2BBcl9w6oj6ysFKIdstCWvWDtK07SX%2BrshFuh9Hl70Ev%2FYt8lIl%2FpLFlKlH6rHzpv%2FrD4QxgGar5L8XCWdZvwsuJl1Iu%2BAk1EdWQdFjimXZ3ZPvhMDgcAdvqdXklnVHEDz%2BVOAweFqylqS0950Y68W3hib0Xtg90r3NlUjR4kxGsXUJSyTZBhTofRpemo43sgZou9pRZTPYfGnlGeDlwZiK9qA3gLOnkMVzmpjFj2GzuMWth0iMVwJSH%2BlmsP%2BcnvplKd8fIP2K1pO%2BFcOF0cyF8vDm6WQNc7jM1prhLzROEzKFQ0PDgFiMZOkwH5AF3rPwEYg6An%2BUUHp55NsmR8yP87jf6%2F34reeVSfQXIpAKNa56tDUWByj5g%2FBrbEmwoZpiLO%2FMXJ1ZFrfsvfc2ou5Mhvm7tCvps%2FWQuvRxfmX0JX0I0%2FGrgJ0bCrR%2BTdkNl5EkSTAnEo1BdhL698ks4TQwhc%2F%2BxgY6pgHTtWBW2eGbi2%2Fbsvm6MCMu28x5h81ur79XeIxO1m8uhxvpsun5nLk4%2F1EDgMvzBnF8OZMdpBbVlmGqhtquxFovIkuhlHATLpA0DRA0su%2FOMIqthFQq72ImVA9DTD%2B9xPTFhxnkTEKmUXE92zRcXsbJ1lJ%2FEMCTQ3rJB6yCAr%2FFr2wwttnmQZl6cjan%2Bk09q%2BmNQx7aeNjhBeS%2Fg%2B4J2dPS3R4vde5K&X-Amz-Signature=757ba522a62c2b2e0b8a8ceb4d9ce2ee5ef286e4af292441719cfb4bf122c622&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

