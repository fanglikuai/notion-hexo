---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZPUX4KG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T220054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNGtIIWIsKgqHH453edd3CxTBoJEAYvAgVoCryJfy%2FsAiBRO5G5j9u%2BPLAaV9I3zWJ6Dzc2%2FrZAKi07d6c9LG01QyqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMR%2FEKSPgE3v6zkqmOKtwD4Fy2pK%2BUC%2Fr8R1ErY1iIu%2F1P9fKe4TWs7UWeuuBEukUbmBJClDvvkHKsQHKmaG70Xr3USPrI7nEqUO6XjMO6GLG2g2qDd7%2FynKSlFQOcVwmFgme8Hq3wDo%2FYCAXfuaabn%2FHKuSw6Z5dJfeMyAoi6kzJtXO0OoGuc7tih2n5I8iOP1REbe6%2FsNPORLfUsj5wi%2BIMKIGI9HMOj%2BSuoFWwJlgxA8t9RsxrDKVozmrKJvQ2lE8mQEUC94Nf0S5MjHy6m3Tp0EU6mrB2b3wfn1sgrpCCr0InHtyL64Kxeqmb0EW%2F7GvLXyucKZjDggQOXcmpv9rJAWFIKaAvfWO1r8S7zNfejAYf%2FzDEQywHNNVmN5Q4ujR87XqKRR0sW9gXxCmOnP4pZcXYzGUlfcMrVJSyKtbP3pi%2BBGfX2dctsFy1Xix9LCh5qMPV%2Bo9atObTZbe%2Fxqw6gx1g5fuYb2KHxzZH112SFviUQjKlETIuOLqZtQY7%2FTo%2B%2Bw1uVOcaxoTyirEPD2fIhM4OLFN9rVJnghunSQqToDMgLRaPWGwkXHbjtHzA2NwJ0AD1wvjitjUqf8mDuRDfg1XDKBnMNhhc%2BhEGODhi9pRpedniGXAuZo9tedg49LHgc626bUdffPEYwg5D6xwY6pgGX%2F%2BsLmGy19MpLnfcCB9ZuEGeVgKzgrjC5yojZpqxJtJ%2F4MqhzR%2F2uzvsY9v3sEPWcg7MJoAZdL8pxLgh3z0pcRmRVyTeuCoTiISli%2FzvkIf3%2Fw3nnH69RxFdTBEpfAgE854rcv0WKDoFYkhCjmGXBXDWBXhVZmHY%2BeEr%2FnXI5xqh6ykT4bGpTsqW21xfZ5abdhO%2FJm1TC02zYUeqVbBS4EQmVdyMP&X-Amz-Signature=b86efc8cc1dadddab403f83f75d1b1d04dda76f24d17d01ec47d9ab83606c358&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

