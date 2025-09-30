---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OWVBUEF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDxrjI4KHFoufAxRaSWE8aPEw4gmU%2BIux2d%2B%2BkOxSKUbgIgGWgzruVvChakIr23cTuAwK8nYnnp%2Fn%2BjGjsTkbK8U%2BwqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxAeZhnN3q5IkZ%2FJSrcAwmkhDkCXJz7IEPTgKxnM6ilFw4BYmGR%2FLlu1NvJFL6UZFBxpDSSZlkLP26qNS%2FQj%2Bq1Y13o%2FkJdAuqAm6OHKTQgJAx0zXbL6dNd6zNJ9pQOehXJ6OyHeipX9KbZZL8%2F9%2Fb6gZchMF4zJUfMfM12dghxxTBU%2BdCgBhkzcxfUnMwNSlPl0yL6HnnbngFlxseLznyY1q9pXdL6ORLKzXMo2wRykG8FxJkrOVvAXHQmI7A6%2FWt7f5K%2BBC20m2iwnIKT20zpKZ6dACxXiFFRDpozjDGmc4wBGwD6vfBrLKVR1B%2BOdZTErIJlfLM2VkL7YeFHzH1oSDu2W61TIAP7WXV37PRLuLe0pd4vu7mtGmSrVpoCHv0Q2HO0AKYlrEwj2NARk9qAUtwEivCh1r%2FiY06Hfko8jKwm8FQVkBBFSgBPpnBYNsPME9JNyy4fa7FF%2B%2BvskMjUKxBKt6LsSV%2B9demeyYGSPx%2FKxmUQhxi2NCXlH6uf%2F9Z%2Bhp2VFhNmMmI9ieffOvpPv7ynx1%2BhoiCklRjdFaYSWSKRKqlJ%2B3tGuZP6z4cxg4uMBPjK9WZNZ2jCC%2FDF7%2BTYkVrb%2BBfkqhXtzruHDBVfbO0i6X2WNORlTqEX05V5uzUBNeVm%2Bl4GyBv7MMf68MYGOqUBdBs1RszB6AqZS6Yle%2FOrBq1mPh4Bf%2Frj6TkZWl%2B09Smu%2BG24mC8N6xnpWT8pAO9DTMaFiSyP6EOnv0kwLaP65D4NlCmCVCyNqF%2FIkEZxXGeWI1iTPXOKIqgSBF5Tr%2FlDmoYUh9KCvD1pBZTwEn5frgN4GtWwaBo89HmON0QMIEICkwVBcTHyiOfn6q4iXjXvFkhpODs2Nd35Vku9frRE8LM6oPfg&X-Amz-Signature=8d720b2d4b73accc530d7f99029eb679aeb1de8c9aad25b1a12f630a56a00909&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

