---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664C25V23L%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDLySIwcxum8w056%2Fq55OMV%2FwbDkxawzAhcG0JA7hIakQIgROPVBeREInQ5UWTSs40PEJR71vpGMpNgw3DNfBabb7cq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDEW0iUTDlN6ijlhxCyrcAzykKFvNKl2zCRMYRwhxsNFHkBtzR028lhZS%2FZMS7oyczOERuxq5A1IV7Zg%2B%2F7Rpvc688SIYrFQNqA93tRqOj0D%2BS3IMfdGE0iOExzTTClGXAS5INUdqiSq9Lj%2BslvL5koFa%2BTbtc7zVVKXS5YAX3gzZVZyKMBY2NoZd7Dm9YKFv%2BYaVqmJzDE5k3BcOs2buDKpyhjqEZcrmcQoWGUNP0HrIqafOERUU33pxilDWNwZwyDxx8oKn3WMRYp0l1dCyID8wVvxyl24v7m9kS7%2FCD7Yd8b44QgXM5eyBo5a0mYbe7GdVC2UbdMgDa6l7Qtz2pi72ngVoc%2B0XljOz9fN54TjpR90mJZgahBgBYAbLH%2BeqK2nM3HIi4xiLx%2FaqFTrWz6Dn3J3CfJdyVyVyOVsCOXtzXso9xt5J0orvlkq5j5K8vXJvSV4j%2BvkA32h%2BLmvVGUihyu9kPzbPogSFaMSHwcxNwW59%2ByVxekpzMx31SzQNJcRqQSCr8SlZkQ2gfugnCIqkEazYz1ea68nJw3%2FLYiI%2B2GD6kewyxeqgS6nCqRhOj6v%2BzJ1l2B59c1JNlNZ5JHxMzE9RocPCNouczS%2BguVfei9qDMYJ%2BlbZLCzdLRtdnTQHHmYL%2Fi7YkYviPMJvSz8YGOqUBRgWmLzNbxcaWh%2Fu1obMhez6hVZrmxIc%2Bd0EFyZbuAr0umTCCDjBaqwTwfd%2ByEvuX8tACNRLJN1E60PH6uOngSfDCH3C2biIdyyWGtGwnZUPPPlUX37XdUZNqYXvPFSKTAJdLEwaW6wOd0%2FzPzSw674SOe%2FNTM3bYkLMAyYBm1c46EaiZksHTPVZQKJ4OcEVu1B9TewfjW1Yc4Tr7LFn30RFXL5ot&X-Amz-Signature=729637bad4b5ef402322b5ad6f0830403494b318c8e3dc957db73f86473aa837&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

