---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UR3NWFRL%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDTpmFLCzB71TFraJH%2Fe8Sh%2BpY9FmeH71Tgqr5LPc1yHAiEA%2BAQI%2FgTCCPocPBVuAbDWqCBT5sGOoHsMlU3Y0I82JT4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDIvb62czUzuZNQ6WVyrcA30OUbv%2BzEW5gIw%2BITikcLUYLAOaj3CUFILcItALLYIdtzH4B1GiMBJkKvUIxsDj8iU73ncjwpQbG9b4xA%2BLhnc9r0FLDGaGD86i9Xs6o4T9CjSkFvraLVAWHxSCHwL9WObEr4JEP3rRz7o0%2BXOoL7zLxC7GH5c6VrL9P%2FQVtumvO1kOrT0OcgcSy0CJwBqyl%2B9mgMetGdGw5iBH3m3ZO16QLLVbFRVvVWJyUbdq57WvrRCUitXGLaJiIC97lzLlCSZqHzGly3mtw1frlfOVbyRRWT5irFqpdVA3gZtuOtFlfgRbeiwb1I1BzF7dKvICT7i17c7hxHb9H5QbjJu90VrVgkb9dpt6S%2Fw0unReZSabMB0DVVxCizyRFbNLPIw9OXjetk99eVj2Krp9Not%2FVyCUn4pGXHu1ZyuuW5mmzvM64eqcTJTTp%2F6aZlmBoFF3HtDkmS%2BvokOf4Tzs43NqK0AJ4d5UMoQflJmIkuqjE9j%2BVge7fmsWnLlRgoHw%2Ffnuj%2BWb6iB0hBcVtkCwdeqkBavxqr8uMkMrPs5g2MriM5kyAYqiSq0lXmDODftnPbhT9hjnpwxTXhEEsvwlwB63gBi5qf0CVGpZl2raOzjRTHq4RGvk9cNDlnCiIDlaMOqDu8cGOqUB6BbkekKcr0Juyuf23Vvs3h13BlVjf%2FzPzI6tmwMVN%2F9ivVpg8VSwWUfNVwh%2Ba1q02n1tK9SoMqpVuovVUR63OhDILHf2SBroo8Wl7SD0RBuATzpgGz1UwicQlkMXaveDpXlkqvRuo5%2FF7t4%2F2J%2FtS0uqUjYPVPp0LszfvtJ91fJqmhqy%2FX8qjuBwLLu%2BtbQu6GYOrQafD9Ug9zzqzqCRnRlbdR27&X-Amz-Signature=0237a9cc298102a9fe74125e9f59d1b4c7595de8979ba992edad6a9ec059bd0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

