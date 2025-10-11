---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW62VZDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIA9OJR88xrsHQ7%2FEPkqv5NigpoeJJ5yu0ikG7rJGAr2SAiBDPBmFVi0HyS6fJBtyM5VuOJiVkQMs7rlujnaRD4tspCqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJXqtklNOMCiLioLBKtwDkfx7BTMvJdHZZTYngSdMMfq7C65uB5IRtalbTsZpdCEoauySXbv8chj1ZaW8ckLJotf8qWfonP9Qk9Io%2Ft%2Ba%2BVcV1bdJDo9HjeXDmXwN0odIjR4mir1DWOAXn9D0JR%2B5d5MQ%2BTPfgVtuCLkl%2BjtTmfev7ZF1xjjeOrDQvs1OsONNpoHtoA%2Fbh42hj80uKiPbSRHY%2FPx%2BVN%2FFJJCcQzuEoNItNXK2W%2FyT8iAL89PgVA4dRotMOu%2FVmWLIOwqzRFZ09ig1K95T1XzmvVbY4LatKiVWGvCju%2BcZpiGnj6VTaf8fqCtRJdDZGoypAMlZd4O6A7mMEbVDObyklK5szPcCTdqJqjdiR7gxYGAo7KROoldYz3Kp9NxN2bAN5ssW3vl9op%2B27KR5yWbApUzw%2F6kQbyoGptskpxN%2BEIY%2BBGa%2FqNmqdD55Ejfrqb5OntNb0iKA2cTfy2b4Pbfik8qHCdYJIm%2FspBzmgHSvLfuE4jlKgVyb3Yeqft9VCAwIb9veQ760STe1N9DBZdFpXuSnq9m%2Bs2An5pjdp2bl5L0%2F2S5zLoW5XlmrlXx3Jj96bALRsBydh8xSdVDyKNvL%2F%2BcguZinmC%2BVuECHgQrP9%2BShifcpJOm04v4DTLrI8X4Z4Qgw94SnxwY6pgFKPMtoiISfZunNhTAIYFzci23W8puUsbkL1hxiaUdR%2BYPA4bxH2qhpnwPWKpKY%2BIUImhxj48q%2BhFcnjL8aEIz0ZoiOV9FvyDU5mT7BPS%2F4D89mGfm4LpFdNGMRrSB4sIcUOTiucq3GA99VV%2FY%2FJbGfzLmKewxFkqnklLg475QVUvph7JVK44JFKrO2vvOlRDsBD5V1kYYfissfwerY8Bqvl2DQ2f3d&X-Amz-Signature=5c67b14c302d427d46516e8761df30329f61c5e9194b73b98f168e307f6c728d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

