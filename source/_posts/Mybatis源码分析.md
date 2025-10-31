---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ZGQZSL7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T160230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCICw1T36siA8K4lt3BJcykH6pQIoDcm%2FUcPfwpUDEwL3IAiEAiymgYdy6yw4KGoOcc%2FRjdDCoyK%2BKevVu4SiBoUk1N9wq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDMOo1M76eHi00Kw2xSrcAx035vNtg0wOFwoVn2vXgyIYPfG1U7HSnUhyyIVYFaMsTK%2BeVC2dZqnDtG3WHbLos6rgkQcqw1u4suyZ5QVKhAattGFRriRMsY6bLl9KqbdUumZnGWlZpYQERqYzMerjeP48mt7j07B7tPBU0WNsJZWBwNG7PUhxxFJ%2FsGI05y3lt5xP2WccyiBGOHMDm6aoKkdqexC5yLtnnCP0Z%2FDVdMQKBcCpiKXIL62pnHiKSfpeoeZErFQwKfOpcZwXZ8sWqCcGdYONPkjhUB9c%2FaZhjmfcRlctnAUh%2BMHfsbol%2BfvQCOyUz3iJw6jFEa%2BQgA8n2y4wpOkbVIQx6vzfOvL47v4wQZm9MdZyRsw99809N1FO74Ig%2FsCF%2F1Q1C%2BT336oJx%2BxT7dCQ1IPrLFsferzQN3NLR%2FcpKposjff%2F5FKp47tmGyq7RKyvLCwMQb5dyqV7WQAIz7Pe0%2FPH%2FI0q0TFYGEyxHurYkeYWgUeRJlZzC2s9pMErg6CtWTaOe%2B2ZyAnYXqP8ielLy6C0oC%2FSZS2aUt6MxNLuT2kwCzdg9cXs5Cc2e2aIdHzNbjJwjihYHYkjQSer3RNKHtHG5Fn8K90KxQqaaOfRMVjQfzx9wtM6yhmqrMcsZ7eBqXJSKcrfMJi6k8gGOqUB9xVOebYY4MN7sRXly5fPHaUuBk4NX2yfRihDMMH7J236ogfttE87RDf7AwF2DLWWEDmhCEVseH7I2Vv86QSVbRHixV%2FghestT0LPT7YsKHvr90Ag%2BwIxVMaa0k%2FdZXMGGc7sUWN%2FLwM8p0778Eu0V3L4q7nbLLuxWdnIqAzC%2FsZNqTSNpZgqLVmowY3I2LoGLVz%2FDl1hZvt5IcVOhzD9ceMycsKm&X-Amz-Signature=b689c53141b879c7b5e1783f267a94399baf34baf49fd7c1f4a2ac563944e373&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

