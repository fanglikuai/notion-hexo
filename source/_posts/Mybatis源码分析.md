---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IB3C2LB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDyrrUHtt%2FkETswBD%2FF7OntmaAuAUKAumzZeaETTm5ClAIhAKUzn6Wq%2BXNPcytZ47aBLz%2FEWrYoXwh8sfsfTAb%2FrohiKogECL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0IGdQKfSMbCuRlDcq3AO8t%2BgSa4hheSTBFgenQ39rzGhVbVJJVsCpXHvX3zO95Wdg37c48LrmsdGdax0MGiZmai63fE0Gnwr3l7Ri5tDfff96qZIqTzCFhZ2EEf2XmlvM87q3VqqtqmG%2Bf0ICtbrJ7Q1HqRFZRqznGAQG1ydKv9QmsQ7ZOrYc0iSY6GU8lEG4CY5QG70xwLhpardt0Sc8b9xTBfAK%2F9gvjAPsBQIkDuhJKqrVWwEVC6khusgNaSCTvs6pWxU6crMmBAbJRx6IiAHR%2Bw3xSJUYhE0RucL2n3GqQeSL2PGY%2BkHIDhWbqJzSJHqqgL0BlU%2FbqOYlAc22Ihsva8LyYISANSqtPpzm8oV2eauEEBNx2PoEtPDcRb%2BdGeyBVwCPN2qVPNWAaIqSEXnkm5KCiFT2B8VafmNQwMPw65SrNDg4k6V05Nxw4N7Xidg7RcSSlJif0i9%2FjhfYTui2lhBcM6t5j9x35YY6OGn7IT5Bw5ZUMeNSG1eoLrRt01FBRK4u18PRKbnBIzdSyMTi9jurnBphdLpt5Cj9m%2BvXsdSEYWHvmBSGVnCviyBmtToWs%2BNRsWoXESvl0Xz%2B2SWARbKc7qV0YlSrSfOn97F42IQXM7taTOSk45DL94G553TcSUhfDK4nsTD42pnHBjqkAdnZmbt44vWujK5f7ZcQxFUm6sp%2FV535lTLOopWxzBmdUzZ1Y2bAODfZo59FhXGNOMRdaBprv9cQ9bESoIJS2ottl1fEBh1PFEqKBYe%2FumBZAt0BsuGx8LRfvSv0g7wIF49tRZKIHhQIQQKXIw8aBaejVRG3tANpbc7nwwEJ6nmN1tHSepQkrwTqEO2JNyM8QQrL4Ac6cLiIXEY9k3ROokZ9hXDI&X-Amz-Signature=91326406f1675ac104b0f9c2eb041b0f70e30b604f7b31bbc7517a4aff05457c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

