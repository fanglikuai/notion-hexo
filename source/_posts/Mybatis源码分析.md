---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667B54U6GN%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEwZdC1d8oSdDak9ZaZ0LgHndlqY4p%2BFJPlTHIX0QtlJAiEAxRUGZLUapYhxsUrQaKKsT8ACgmK5eOIYHC26eQBhPRkq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDDyxxu5Kf5lJ0TDuhCrcA%2F3fEISQ%2BRPQCyD0i2Z2akcyUncr2Vr5Kz3yWhYHMqYKxNVWmYkEbnR61uPu7myatfi6sTzJBASkzDv8ZcovNPmDsuX9mD2FUvwSrMi8NL2CMy1YqzMFbfwP2LKTyJQiDQ9qqNfR68Xpu4PwnsEKiqMYwCoSZDIpwgNWiPu4aqOIvJsCLuIXwPzk1VWNWGOBMRVxg9nDwTdaXDFIRTX2pnzGvX0floKa9o0qsckOI8qNjh0G9YwqwfDUqrDkvLj55M1WguKKtQ5MXe5flsERmFpc5aNJN50RKwJhpEiJLX4ymcwFf2uzy1XdzbOSF4i61zcF2FZCKnGmag8YXMTDZw%2BdixXQk%2B%2BQx9DC7TC8h31bKgSw1FWyj44OmMhQAMeGVR%2BWIoTDAx%2BOWMfy1AbzD9AqwvJvAf5Wpyl5YsypNN8CZmW0za8TIi%2FQANIxs6QnIjZrqYM3qh4xoY8bD3e9YfAFseodE05PvGUUAScOra6iSC%2Fc%2F9uxg4G%2BLArv7TraTsqSjcBv9%2BHHHN64RNgo9sUl4TL4S%2BFRdhV7sUpMmY%2F2bMNhWkF2iuTvEgm50oTBXnGBJbE3lNmNZNUQN9ZVOnG10KoA%2Bjoi7%2BCgq18c%2BZVOXQG2ZoHU0uwfoJTDMMPp%2FsgGOqUBiPBV8F0NboaX3SnDcJnkvkQn0YpxHy9f9k9f3cjLsKEubKhVwhdaE4hJNknH7vwSSlHUkh4YPH%2Fgq1BeHIbPBYzwsGVSLq3LUd4V78a6FsR0DnpDdWbfjJBRv1fVtCGXGZK0HUa%2BeJhwWP3nH%2BDjYyOrnyyEz7XWqtWSZO11xJwkRj9ZCKml7CDs1rVWpjASui2PWslbXUDF2q6Im11ImDuWiJTK&X-Amz-Signature=aacfde870305a5285aff26137755aa84d263e71aac56f4dfffbaa325c81c7507&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

