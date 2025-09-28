---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZW74EFQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCNRnl100aDxHM1t9cnWMdFXaHqjYVlRQzvoOEhzdJRUQIhALcn%2F1zbz7PDXLEfrAdLXnu9X5%2F0l2Cory9QLzvh69%2F2KogECL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxqekgZj4EWuA0B2mgq3AMf0nMdD0qXm6fCDdMzb4ResG9xt7tS4G%2B9Rj7UHV4ykobRdQdFwrdFskk8JFZBTPrdxxQn5OCf29pV44mN%2FvWXMj0ST9EUwqqAtcxM%2Bj4bJ4%2FEAKgihpnP%2Fa4QU4x%2FROLnBDpW5z9TueAElwfirdAuZtxbJY%2F1IOpOzX18NVSbNLfyaqdCI7eMbVzNuLlqQ%2B4A7%2BFiwU25h80yf%2FA%2F%2BmRm93JRpO191Br31gtj376uFiAUdzzj5qwZQbL%2FMPVOC8CnAw1kkgwrkd7aqnfGimzYsnGdXnAYbQHGSOFhsDExLCFCNmtyE97QX6QIEdNR8kspwJ5%2By%2FNEuDqoLJekBh9c%2BNHpF1htHyqburbObxlakOztRn4v%2FNY2cJAchXObC0emUqa9ABmi5k4%2FVYarXA7en2cJucLu2I8YQqQlYkEmvtVX37zfDeZpfQ6dUt5g6pQCOq6H9k%2BTbv5k9We9wgPqb1ohnHISIpfysyleUnolK3CozU0Q%2FL2QIOB4%2BMyYb50aFpnj1lpi4dUfXn28Olbi%2FY5Tfc7%2FN%2Fb5SAzMac4kY2XpYNAB%2BAFShicQK9CD5wXelbm6MWcR3a%2BkiPHC6vHxDAjTxYo1a2hexI7A4wVKa0f1aiqyN%2FVOcFdCmjCZ7%2BTGBjqkAY9DqvluwQ2brKh47%2FwWqTJf6F9FDMw9Z58hzwHp%2Bs6VWjuXqS1ZBZmDtoYhQ7GBMU4%2BDC95SK7%2FvnW0Bi8a%2BpJRi61AfqqUIjHlXrrxIfPk2htLRyo2us8xlbHo7EE4fy6C39q87FXMfEUtMtm2%2FpaOLdtEI22O%2F9Kbfph%2FfhFbNa47eH0VmNav61TpjpAplsGLpIzhAzeecBdFWuSLsIJYlkD%2F&X-Amz-Signature=99d85ef10d07b3c37dc0c83f87c8eafceaf7ff0f01c8b2325237ceb0d6a1de1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

