---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662C4EVL6T%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGtRgF%2FtYN2%2BLAN2JwDPT9xal1yjIxLs5Vqw50gDcCAAIgSwqXagQLzvs3xPBL%2BHhWPl0N5ZObrlSFmjrxeoye2EAqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNRTty%2FrJj3s9%2B2CCSrcA8HgIToeojlE8ZBS9kA%2FBggsEQ8Vq3D1NzzCfpLhH0mQoe3bJEeaUpqRbhMQDDQ6vk2COYCt3iLxr5PYYM4FchvAVcaLMEP12ZnoO%2FGPsJukcd6Fa5Ld1ItkF7%2F2YTk5bkNmEXpat0fs6gfKEfwDAwaJ%2BE3tkK8nwSe%2FTsCN%2BFomS36sfHuXaNYRh8blBDH6yPv6Lk%2FHbfixSwHcPxpdJPBUysnggfwj%2BoXyViShi9e1sfiwgiE7Z5ZUyFw8OpDxmAEQvQosxo5nTyRSljAq4SV%2BMSwpnjUt8%2BSREpI%2FN0y3D6c1vIirwLemBfUzWHFyVU9VI%2BgRNpGSaTBmN5hgqJLI6ALUJ2RLJ2A5XHSxBSWNIgB965KdPJc5MIw%2FAwB9RvLu%2BtDqeKKntJZNV8viEroFf6ifUuSqesPcBQS8w56OJOdLoYADhhUuxtLgUQYGciIv1zilWuqO7%2BDtvbnp6VW%2BvsPy04zgFQ2eX2dDeylVVZoSay9GvMk6Oj3Jl95%2BJ9m59RmRO6%2FrhRqUTrE8bNqTchaO0TZdUTYVkPAQ673WqLLzs%2B1jYVsWtt40PnCZWEPU%2FORNr5%2BcD%2FhZkzzIJdzdi%2B%2F4OqX5d6rn2h56dwgcjWOp5wdlH9a%2BG%2BVoMKK7gcgGOqUBH9R3Jv6gzKXChmBM2URU4htEs9NabdNts3OZdWZ2m2qpn6vkyoghXuGRzPYG%2FQB8hFbZZRPWPaqutLf1C9Pga6%2BkgdRZLxnQUchqZaLX49fnt43P3zAT3UsK3Ma7FuIfmhqt07jqfe6WZv4nlzEriE1A5HavBeHxoQ%2FbMOdS%2BKa358LVlwAv7qMKZVDRPOQL%2F2yqdxUEJmQ2dRMx%2Fh8Cqzs7WjH%2B&X-Amz-Signature=526843254809c067c9c95182734304c434c56e1bfd77b11e40e79a512e480a14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

