---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDD2LZBO%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCL2NEao7jLZigIq78ypr%2BHdMC3nwcsVASunjoEflaqQQIhAN1pRAd8jlCdm%2BVN2%2FpqLyAjSrA25gsR%2FeHHcP4p9hhsKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9wnnteIA4bh6S3bcq3ANwnYA7dDjX2UKNTte2s0Q6l%2FLPgym4az9i2AdRipnNWs5M9607TW5cQq1YuWA%2BJ6ZxifM%2F47MfIT0F4yMLcM2WbEYbhUaSGtxsJXBU3GK35bPhRFDuJwVY8iy5466tHxOUjkFbYOX%2BQwp5bFvEJyguxYTMqsXtGds8O1AJMOhzVIAPSi52cngt6oNnJGrF8FG2o5%2BjqI47GNvAK0hQG4MdHZ3Mp2XWDoHDvmYTz0huY4JnEsR%2BesrrEFYuBxBvLg%2BV4qtMK2VzrncP8NFU8HdMvuafP47kxzh%2Fi2L9tPIjTaH%2B7ohbs54VOJ14HRdmePbu%2B6enIp%2F2UjPTc1ZnJuk7NydOM6VZbZlg29WqGFPpL%2FMVNNEDXYmzvohmZrp7GxK0PB66oUCIzeaCfM8Y5WS766qAy7%2Fm8aUqBygjV2F29s2Ey7M9GXVDXpiiPSg1IJU5Iffw5bboVDqLIGIy%2FSiLOlUS%2FFq3we2Hx783SUY1QYhxlVyd4T1Whuq3qJIKV01gj8%2F7OIbqgHuxppLq8kKrehXPPM1OKYGu4bP2iGd1nB%2Bwp2ehhjbnXTatzTKT3e8TwvLe%2Bc4of4nEJTSHkUprOH7LMX0uZl7gv6hMvB25Xegz9HyAmhV4ny%2Bj9TDb46vIBjqkAe5UyzLwSMB15tmg1Uodb27Q52ueNshpTgUTTqZiLAG0iufCSFURwzdJEkKRArg6F3P0FfcU%2FGmsSFC6O9YOsrSFqnl0a6C5MwFT5eXL8uFYiyPrlUE6bZdaDCfbSLXXaL1RpRuP65w6t9Xq%2FAh0GVNVrFpuDhxVmiODqssUXbf%2BmPuv12zLW3IvnNxinedmZ%2FFhFRmtmKCOItwvaPBvf%2BVqBTRt&X-Amz-Signature=a52a65cbb6dd07e926b374382dca12851bd9ef060857b749eb0991efd4b0e40f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

