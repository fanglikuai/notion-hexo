---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SE2B2RTH%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGUh7JoE4eMw5QJom2HX%2FOfZwBmd05tgKacLbBgYG1RHAiEA%2FqoBXT5%2B2C%2BqZfWLkmmKzuBSaZKggDEW3X17sEHcVloq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDAsSCFtQsyImHSKCSCrcA0J81Y9LM2Mb01AjmR6JIz0b7fLeRhxy0NGKi9VuytwRBQ%2F1NqBbyKkjVNiLgdsimgciCy4arxrjHfuXuqzSNwJ0sA%2B4LVsy18AFUZLogaUF1Uszz%2FAf3yPgop%2FjNnxjlmfV9nQovTW3dWEx9RI8tB%2F1Lp6Kvr%2BJPMh7KpDmMQgagAK9e6oP8liuI%2B1dxia%2BJKlFF%2BTCWlZQQQSHBxGoqpVfq3IfRVhi2tSF%2BscS%2F1JJRMacSrEQrpZ%2FzJjQDvFCOkdGzD5GzQBMYuSw3fCGGR5ty8lBwH%2BR3GOioRqdEIzfiu%2FVxtWDRwXnZjKB1foDXEMkHMlIbZun72xS0iYyp8GRMVc%2FWQZOnNVDsMau%2BkcgmbbXo3ltcmjrqqhtYIjQc7L9Fq9tZ5Ew4cwvqGxuUF6S8dbQ0E7q9ONSa3kM0oPvlEXY6eW0ABLZTlZC%2FIaNuO9ofUJjOtFn2F3fOr0CTFPha4%2BoInUuw%2FZ2oKJywsWZDNwqNviw3a3BQJkwafUHwIbDkhnFAqa8cfKLH%2FZPCyCAIjTYHZ3e4NXgwuF4HV8Hz%2FKL%2BwgnvOgFP7Zmh8aZnQ34FgCYrOAq7kmLJiZ6Lh4Ab%2BF6B2Oq0qSUg75qM5OabwK4H4k90YMbjeyWMM%2F50sgGOqUBuvMFmP%2F1PPWaoAqOS2BA1ZdzH1Hk2Kqg5viQDw108rSBw2XQFQSRPtsU1ekSbC2O5hNsJKAjIF7Kh7%2F%2BorJmdJsUMrN8o0G6zI2qiGhoCReJQ%2B0Kt2Xxs5xNmsd8QXKHx2Wct9N4w2Rk89hGttb3u2dNDFKWTQo77ZROSr5D%2BPwsy7nqMz9zkeiiZV7HfPuBMK53QxgS7egCEZkiJcPNY5L1fGQ7&X-Amz-Signature=a8cd0ae7b35956d37cb184b9edec668aa122341ccc0044d6c146f58ba64f7ec6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

