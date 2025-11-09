---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNHK4AU6%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIE5gHKmLl9j2bdIcfFSiRfukUqkpK7wo8jotqO9SyvjUAiBKzNs%2FJxOlfVbyEDvJ7CX%2FKger3a60mlvf%2B4130qPPpSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrkJOu%2B%2FQWkIL10eOKtwDzdP9v092ErMDXPNTPlj3qXlThiooP6vN3811MLREbBPWD9Fz357Y4%2FjDX4h%2Bde2DIFNpqcqkaiVLHdXbD4Fu0X%2BnZ3NtOsFrFfOoU6sBd3r5%2FxZgBeatdPjufUiKCKYJCTgi69SK2NUrRcKopzPnyhNB8bCiLhqkGqHxDgHKpEnEb1A7topS0FeaNQ%2Filk9hSBjunR2I4wBvtqFifGlpMmEiz12EFxu5U99JqqddRU7niiFw08C0OdBF3SL3LeutWxbxmb653bfpeWTnurV1l70s50r%2BENB8Rj4zFu8flOIa0UobN2dRli3Eb%2Ftky7f0q19Sx6P39FAuza580WZqGwl%2FKcS%2F%2BxQrg%2BTkpjzgkpjnaGuQpO%2Bha6gQIXeL2XLjl26GWosU7HFny5Z8d4B1Hh0grYkvi22bLde4Uauc9OYcuBV883Qi1mdBlPxdwg6KE085mIUIwImychxStOcq8lHSc4GceNWYfK9UiDXRrxfShC%2BgGwu%2FjNGzaLkVn1PF8LSyal4ub52U6PsLnKL964uNpL4raUWhQnFNhSJOFutmlN2tWrHJ6rqu6Vpwdlil78Ipw31w9qWinx5fzKCLgpw6%2F7NMF4apQMLFowqUajSGP7bwm7O4k0DTWjMwmozByAY6pgHgbkeutQmsR%2BmUEIZfS%2FkB6rFWEnH37Bhdj8J%2BL8TWHfo8KozfacnzuHcuCw2zFOtRZlx4N466dxIQO2IT9DXFO6Aw4rZmo%2Fircw1ivYiOVD7pU9FvKMxNJxBclBrr%2BwJ%2F1Av7NdWO6WGmYcrO1yLmFLK5SckoyXTBg9V9Qu33dNfzonDmYngBj6MJN27SUeZfREiMl1eXBSDvTxrOXBWZNGCC1i7%2F&X-Amz-Signature=931e811e8861861576a803f8cfd0cb4ce6b1fa780e5595a93a8f0871e1e3f639&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

