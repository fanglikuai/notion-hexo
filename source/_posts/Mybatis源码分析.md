---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2L2APNS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIG6AonfCpgAyXFRdbSl1Dr2z5Bf7GwQY3Hus41MVPzpUAiBTaK0sUlbEUeS7inUBZXZJ319aHZb8bvETOUaYZLsdrSqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwl2mxYyPxuMNERXBKtwDvsZa6sQZGlFECGeoZ0pNBgcjWTXoUzm128akmxRPKKMGPbQOfN%2BJ5%2FkgUSUmzpBGgnzF%2FzLE%2Fw5RhnfEs5cbkC3XQhH%2BzUuOE9UohXZnHXoeA%2FF5U9LZe8n4gOuWa78zC6EYYknbR7qz2lHqkNjGhir0x8S5j3QjJunqRzJ%2BTrexVe6E6ecmeaVEHiBCG7tJ6Jbp8QI8B5dD6vHswbVcSgGBCvHO9CEK8wLEZBIMm76gU6CZrNOm2QhLQIdOMbXgTT5yZSqNn9iM6oAruD5xj2rqckKMZ3aoR3531HFYLFB2yvyaUXaixt687%2F3H1pb%2B7HM21yvPvMx7ClRPm6M4p%2BbRVTuupf8mDIhlVv1bN2XIoLOa%2BoHznTqOf82aew8XhMTVSzU4ZwP17pNkloWx2BKJrYAomxHt4khHbq%2FG8%2BXwpAbRIx2QmN7xr0Z4wxY%2FQzJpwOALbYVF6JHRt%2Bsk2ERgo6btKMs0JKVkmAqZbpHpwrXou2ckZvhH64LJZDEWHa8%2F1ZSMhO7%2F%2BfAB65Q5PCJwYVgiGbBkHj1%2BNBOSpAoEPBqvAz%2F7y6VUg7sfjmzJMqC0k4lm%2BkD3R%2Bh%2BwnoZ50NUehoE4n8UF%2B0O0TYgaawQdTteFx2Bn%2FasjLMw8MT9yAY6pgHKy7FgOUVb7u9lZmENKWSVS6DUghfpoSE5LtXd%2BUdqlpD93FFaTSaWw0iIropWrT0wZIvpruUoOsysQHfcU3WUqa2MzvCSGYYukxR9R5a3s10%2BCyQr21jGByaUYp9OntnKNKmbsAPNm3Zw02HBp17W37O1mfUy4m89I9cVSKJ0KbM8piZcUc5UYwmFk2uQV3CPf%2BUcd8PnS96l9G5jQyvMtb5MFYST&X-Amz-Signature=742293f714c082dc14a835fc2f332a15a6555e25d14edd109f59725b2d4f1bb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

