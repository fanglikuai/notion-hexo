---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS5FHIFB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCrdgmePFiEmDuJgmAFp1fACvk2TylrH6T91DDZlTDEygIhAPH01yiFg5HyXTe93LBPHNX3%2FHkvd1dqN29M8QFwoX8rKv8DCHQQABoMNjM3NDIzMTgzODA1IgyOloRNByJI754rSeIq3ANc0d5szjtUHI0mfzFPxetL%2BR9dmldzwU%2FhvjbViNr7IJotZZHT6iYeVVy5HOMOA9u0PjuxdnJlmKrUtKmGinn5o7zsB2Neas%2F2G5DsQHkfjd0T6JdkA8FIIUOnxSQMHYs3o9GKJl1ebIHiCwZ2Rp%2BZB1w432WzA3RGnvRa6YXPZWo2AzYnzA%2FpyapGGbDkLI6mKQ7eQ8cPu%2BL2ZRlUWbL4jOZGnDrxeyzUnsZQowS3Kelz5Ixq2hbo%2BIlz%2BNTBurtKoY6uBl6uQDUtchiZS3W0eXuU%2FrrTzrXpaHU17SpRopuK2pTb%2BI2gR96261yBigWgufrMHwrMS6dWEJuv1bLMtlhDfrYllnciNkWINRoKY6IAodLWCfTV%2BDTtY7fcCIsWyuSXq7G6nM35fC4GlnnlazJUsB8HK6AdR4CT%2Bo1HtEJeji0%2BoDfuXb6ZrUDd0uyhM1GG5qw%2F9%2Bgq%2BApr27E6JOUJDrHeEMn44bSINLcRRGPMyDX3ZzW%2FToV%2BV%2FW4JOnKFTHCjrpLZU8LpEsfbPkiTYacxFavB8kl5WMeKH47%2BE5MEwyFv2KAFX5FdOKUlXX7OdGttQDOv5Wt6%2FTKAlbT7XvwQBGDZII9irDweM1tFUfKq6SRmPGCFG1BiTCpn4nHBjqkAUb3CuEcaS%2F2fqvYMH3zoBjzGmn4wpYdVU2C9Y2xycMJV2vO6dz3iDOuIyxq3aHHk7HwK2BROvtsqJXeEeWO7Vkv181l5lkuHZwh4JQoqMaqUehjsN7dVQXBWQChjJyqyen4l16GicKuOtcrz8uPf3UeMsRJJfZ9lQP4EPdyse6vunofzdFQBYGielobSaRJKwqVRR4ROjTYs%2BotDS1%2BnNMifETR&X-Amz-Signature=577900e180e0d592e2befe9193fedbcf1988f4fa4815c113bd865b1e18e97e77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

