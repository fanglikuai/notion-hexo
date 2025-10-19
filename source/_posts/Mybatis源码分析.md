---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRWGXZ2X%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQD2QQi2XSipIlqPOV4rKgg%2F6uQDpkpkC67nitsiVYj%2B4AIgbFOdTgxFJJpPsAhegDfRJO%2F1liRuoTo7682WQiFVvrYqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOsrp303ImD2VLnnoSrcA0qe7y5GBjSzwODo7RhaB0%2BJ%2BQlSgMPObk5Yjhcqm8I2mUFVPqyFE5E9NTqOEfVWxF1WUDmuH%2FTyx3Y3uilBURzZxsSN30hekJXm6hY6LaAhBR78bIM3juymxHqrmE6%2FkVZ06%2FtUPgc285fnDvc9I7xBUBBqUo7FYVz%2FH6DgZ58JgpZVVK6JFUzmR%2FUrL3SVK0BzaJOlaqCr1LCA3Cc%2FF%2FX46yU%2By2sKGasrgANhnW0Og59kB5P%2Fdlcb7Rc77f3NmXdWbhWfAzzeHXfcZZPzCIwS5aCQ9F5D2vCGHghPpCcn%2FqnNdyHaiwqD%2FaqEHQeQIv4kU8D8HA4wZFnTRplHDmEEvdR9RmfO84BBIITOrUM1bDfW%2FJsjmpJThv5wqxsbi%2BnABhkMLIaV3k4AKrey9R9i%2BwbajM%2FMnIJ%2BNd7JSguPMNYIelRZXg94mrz9mC2%2FwV7wBoLQq6RMFdHLNuJ4hNK6vkK5PaFM%2BdDzqtcNPEiNWsSUhuxQLP%2Bk%2FQoC5VXy5Jsv%2FRnxKSdFn8ZxGq%2Bri%2BfJeIWXQYJtGMwCLdtFoGJMBlVLhnl3WgsG4eAnzZh8QNxtF4kHfEAm97dfg8UvPhTq2c57f1gNdTVFfJHO6DSZSSULSug5uRY7%2FKhEMPeN0scGOqUB2ISPLraW00SWbYx%2BGf%2B3a%2BszyhLaU492BA0aQpz%2Fq3eHgfjnxBWmIwlY2SpbGz6BBV2LuWsCP8ihAH73ypW%2FrbuKFMoxzPlEHP%2F0GLVhQ1lSxh03mSErV%2FrC1UgtHKwT29ZcKggh53w0N%2BuRqdwxn92hY1rJbdexHvxNO3UDrcv%2BPHGQsAFcn1ebc5kANABh9SHwGMPwQXAy0jAI3pCVr707UZgQ&X-Amz-Signature=a0ad3f5ad3316d5c1b055c932a662211ddef7adf0bce9e9a21a6f2e155c83cbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

