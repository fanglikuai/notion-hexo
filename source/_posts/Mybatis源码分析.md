---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5EHGZP7%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEhnuyX5I3VvXEPj6MMs2JMrsmeBEvI5H74hGgLdZmSKAiEA5BXoS3rahoGckWaEbFiSDWqtgAcHJKFuzdtJ5KwxyrcqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDffmjDKqO6PnPuBYSrcAxnlpAwPevDsHEmuVjpXmcJhqWiqB6zLDmNQ%2BOsPhndDf5v4x1ucVeSzAcYLXxMxvAXyMB8t0Z2tED8ahFzQYkRjx4T%2BTWRovhOCy0jS%2BMKlrUKKcI640LtJEhsAEILa%2F6IJbCN1LkCo8CEaKWQVu6eD0B8fbkEuRDs8vxWBGUxPXmfN97gTApQ%2FrLyd8KeKScpOE9XnjJysG8PoOHQt0SwpWMqlXeW0wZzyTdnKwcZ%2B2u2cILedQ1ecGpdVgbxx4JmvI8MS4LFluRzh%2FJXedC37KGCYXRYqFLiXzmXizOqxUklN5pu6bmFyYTIvqMkWtjsnif4n8OtPb5ECzkeydg6c6i5zbMzktTvRL6%2FqyrASpbYuqGfBnS1JEMHN7q%2Flb58wDgoirzO%2BGAN68uqmWFcqOIygABBFiihLEGWzbYNWiNI1disLCLLWsjE2WXWtLjGtVJ%2F5ieP6SsQlrbp5C%2BhtoRtsvhZQwtTXnoiyqzxCjrXnm%2FCkviKtWHA0F4mjDjteJKXsFvOYH6Vz5E14Md2rSQCm%2F3XnpvScE6ZnOE3tZ7XT2Dwv0XDeY9oMz2vx08RPIo4KVlN9IGZVw5EgBUgjWSbcFjYmhXQsdawX3I0lLV0RoZ%2BIDE0aK6kdMP%2Bd2MYGOqUBg%2BO3E5P9csDiLLjP3O33QHycWkPLaysVMjRj%2BSJjFME3o4V%2BRt5uEjE5YLCroD9%2BNkqgnrWBW0kuzSqqHJBRZe22hzu%2Fv8Q0MXxTQyu%2F%2Fth7G20vIVb7TkYGoSjI15D%2FUL7X1WYYZualAmca6xkNAJJxIRB3pn64rABVi9i8D6Ug4IFhl7BGmeNpg2YAyDFdrqDw4gGaBV5PwqOakJtzbAJb6NXv&X-Amz-Signature=c516be7079db1c1ee2ad709fbc3de501ab2e76a0ae88b79a4c206e7d92259586&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

