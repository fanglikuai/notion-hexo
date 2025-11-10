---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665N6WAIDN%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIHHrD2%2Bu0R13SBuDbZVzkHK0k2i%2B4FQpxxzzA8K2QDGUAiEAji4wxnD4dc4ZfY1OyXgI7mRSJUKeNXtS7yPc0gdo19Iq%2FwMIDhAAGgw2Mzc0MjMxODM4MDUiDB1P1JrZ%2B7FZ68vK4ircAyRNH7yhGedpotG8hSqqJkAT%2Bm1nEDsSDUr3eVA0dw05NHNqFHC%2FV%2B2M3AmXjYwuLisoD9w4YhVW0ZE8BJXyvd%2FL3H1DMcLL3QoLfM2eto4EF3Zjdj9Sirw2WLMPQtELIl1kDBJJrufDB%2BNp3y%2FoLKfdT%2FgqYdezJ5E1iiL28HHodUWXMgo5n72iY0jYrMi4Z0tXsws1yXcacvAjsk6KfiIXRmaiDhpv2sVY4HnXuNI1S2wqRR6hOptr9YHEEFdaI4W5JHElhhqmJthaoYrwMU95IiXjeUlwtKRCFMGsYq5N8NB0Nxvw3J4uKPrd8gihE9Zg%2BkGXUnoAhNTb1MJBYML7Ii9SXtIjpke1GKwFfoOHkyxZ01TBtIZFCiigPtRfdxS5bYEen0Cfm4u2AdMMvVhhIHtxGybg2Y3nOdAsG24tcJUlSKjK1stsmczN%2FWIOKC1prSMUDG5sXZH3t85rlK4C2JxyLa9A5xXpgvvzHRb8GOJJpCl8zgiHT7zTYuQT2MV%2BXubOHyKhIyKzwsd92E9TFKD3uV8x82xPOecrsRJnlmAaXqE%2B2OvtNn67mGRZZQVHGd43TJGKJM3K256YnOfx5BaQWIW5HWAagyvysP%2BDM%2BMfjKxgvBPQNnAYMLmpycgGOqUBYQNLTo8Kgc4gWagBJuma7IjqDUZ9y%2FYSf8R1o7hFH6223VrlU80rR9ayR%2B0TwmYshYol5RYol0vzk0y%2Ftt4EWvjXapEni%2FwFkl%2FClpVvtRfsPn%2Fk%2Fz3SUrtoViGh%2F%2FO9CvCExPTCBKnMuzYwRL52WQyBwE5XMhWz8jEZgziowpwK961MdcXVh7%2F10wxNIveAzzs0vceI1a7QwUqKayNHTMlBUZNF&X-Amz-Signature=b2339b544a482301a670ed517343d822138a687e08010a416e4b5db700cf3b58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

