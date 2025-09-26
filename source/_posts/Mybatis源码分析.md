---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE6BVMXV%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T080150Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQCPACzokJfpANSscZoTfEhDXHVCyEepbWv6wKeVVX%2FwLgIgRX%2Fdh50y65yNpvdqViH9c98lx3tXpOcUwLAISTkqZFMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMKdavrYSUZRh8sXNircAwOtT3z4rrgHYY35sfU9b7Z4a9Ej7REZGuDRJbnwYyUOWtyiKsYY6XPFM5nk4kK8YwFq4Q9le7mdtJ1Qc3x0RV5Pvfk1%2Fja8N8JMuEIAEuxv8Sv8VypP8bPkKLellBlL3r9DOwy3pZwlIum5VZqV9O2zOOKysdFt1WQ5kcBS5ZRu0dUOujjKX%2BQCGr%2BiQyMqloigJNDFz824IqFdERHCqvCUg7a9GieINOGU6DagFbTSAr8U%2FhKk4CHQaIbv8hMTUvNbHNgoM5URf6glzi4QUBU%2Bzz1hSIDOHHqigTluNIgqjZva9MjR3V8%2Fn1RSQKOY7ZHfnetGd9jdG5EOQOVStqlLfGrKZbjPyetyGnadi611fWLxoEb1FXx4PMNDUCvCEAv0xRjoshES%2FN7LWMmUo6%2FB%2Bc%2BrbrkVHejQTlfVdnl%2B%2BWcKEZsNdZXxQPLA%2FVKW8V04brvOB1lYEGS1SsiR4gelldHFEx8LoWqWMhFzWAVYlnvTNVCJ41Wlv688OLS2aJr4%2B6INqP45liSwWqwzMSnefzSAdHA10hHtMX1WMUE%2FB4l%2BypgyoT3jBN08ImWe1Jpi4rtA5lIo5S5NA0wIZWTo88oSMH%2FGby1f99wYUfGGC5zT0jY9KXemIBfCMOH%2B2MYGOqUBWVPplpV3fKeFv28H%2FFy7FxFeCU6EiREAMrmyoKSm4E46rW2jYZmX0pUo%2BujhYV2i90JVg6gf%2F7IFUm5UOX4tvxmD%2BqcNbEujdOas6m4sjNUwNl8ccN%2Br0spNC9h8ZB8PUZ8AoXwGVUssY1mlhmBs3%2B9CEEn7g0pGJs4aFBdWJZTAZFHyQo%2B1UWOnS39wrwjPRKWP0h0xqV01ACcVM08tbBHk5mbg&X-Amz-Signature=83ffa81a4d8f9eae1cc35fb1f65611941e7dd4a940f249c9607e939ba91df9ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

