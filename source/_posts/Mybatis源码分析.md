---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQ7EV2IN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCIAh%2Fmxhk2uit5ykJd44CHTPZ93XCup0KH1zD0Gf2RxePAiBbQqrl0VhNfSz5q3pooVljfmcFpx2ebBtlvvveCGkclir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMf8KPq5BT86H3eF%2FKKtwDglOPJM1QjZuvwgVthhfM4iWCdrAs7xgGtsV%2BzMMHdWPI8thNeg3dQ88gX7Sd2EaMkSskRtXfbYwpEp2D80w2JZSjGkN7VY3JDAFd%2BlRnuOmJ0hTu9odBQkp2XLfJIH4VuhJ6YOTyXxg%2B3HpBT4LPAb279I2vTKx3u5HTBY7BdOCCugzb3osuzlBv946ECXVBMyqDQfuzs5%2F3c5FxDbvq1fsel6adBrBsjKN99BmSFQRWa358O%2BmideB1Fa6XE2lmJnY%2B8uCM0rBtTLYByXEL3NI6NFR05p2Qeo1whcWZ%2FhaKI5x91%2FUpYmEUzJyx9LzRJ1UhP45MSImgMezXKViPhhlIyenvilX7g8MnDuEFT3P1Hpwx49LHUWnSCjNMX26qBlwnCx9ydlBhBBT%2FTuMP0q4YBs%2BEG0T9W1cGPiUG5wdHJLxi82WvqvzND0jFu5wrqHbj4zOz5L%2BgXsflOnvAUBFCtB%2BVrXnmqEHHB6iYt2Gqm%2BQn63kCGNWN1fPQZ2e%2F9PF6BixzrfrJsKgzB%2FJvS%2FY%2BkAqdmoKgrkDlUHH8RbzINE7rwiUwyjnfPzNZ2CyL7u%2BiCCLXZHFvWZd8ovElrxIubFy%2Fioafh51C1itZ%2FyxNynjkrB4SAdbeLo0wzo2EyQY6pgGb3uMxYf6K7b1OL5foOMKlhddlkPEzM6epnQinY1gtuD8gn3mx1%2Fuz6Y00aw0UnPPArhnGQPv1vApeDAQTSZE2shK3EdIXzYUrqab5WXSv3PC1hf4LanAeCtSdMKGf0gpV%2BESLUA6e%2FVPTOYr78ih8wUqLKGza8C8yJSwjQqgaj6075nu7aTxPI7DWmxPNulkBsPkKpHwUsJixu%2FlxxQUYdFGpqSEO&X-Amz-Signature=84c196d4585f82f8f8d87454e181384d4930289b94077539eac634d8a9397f4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

