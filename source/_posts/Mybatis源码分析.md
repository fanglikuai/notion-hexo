---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CSWSQ42%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCYJfJ9MI8FNZRTga0NPceJ9%2FQcGfbxR10N9KIziPhBaAIhAOadWUhUS0u0dbtfIhjd2c2jVFQwRbwCghXV84Ob%2BlSoKv8DCFYQABoMNjM3NDIzMTgzODA1Igz3l3SglkZU2k%2F3Wn4q3ANcYAAT1rgZeKbJ3moeYYdwb43wzRJFug1f9lEg8gvwUVz%2Fx8SUPV6B1bBs%2Fz4NVvFHbfDFLCXuB475KA0fs0%2FtVntsle331Qp4iUgDEXVjwxPyBkNJ3%2FTQsYiV7qDiQ15Dd%2FhKP6G8dneTWDfEtStYqTks7nRgsWjkPI5L9%2FjiH5Po7TbOAbXrGKesL%2BwX0oLUIbAg4QcB%2BKTfgvl4%2Frvjn7AIOMyxwTFlXx2EQ7nqdcqf4ygkNun5KhSLuHc%2FtqUto2M21cfuQOb3JxfT8JTSTviD92AK2Hkmkh%2FYZGUkPnFuMRtSnuwzTD4hwEa9M058vVfQqDQuTZ%2FThEPvQk%2BE0z7jOiW0CW%2FH4D6r3%2FdsIRzCojtVUsdCLKuMnvU%2Bt11yy%2FNEGo9TSnObHq5LhXFGVQURu311Wn%2BZkYTr8OslLRz4VU%2FaJwbEFTP4v7Ns3EY57HXLSQyjGl84dauwo7fWDGvKRROWzDvuLKPCN97C%2FEM9LxQibB9ERXouk9gPBHEnzackII67I6XJcz43gvL5FBNTCed7X%2BH2J9pcE60NfD6ewCOMptfLGFXzLP9RjbtORqxAoXUlrl2zHcdcExtZ0WrWgPc2NlBtqw4L6N17K2tX6DlUgRzFq8zDJzCajOzHBjqkAUvYROswabIwla%2F6T6SyuHebcR2f3eO8GHDSKuNkvqbmUb%2F%2B9aBm09R30LohjHaHl0PfB0mVyghJbw%2F0SwWTyafan6FwPsnrBtoG4F7I42U0L7Lhc40I78y9epEJts1Av1yno5tTfC6EIBhHSWDa1TbkS9KPw49OwRj6DcVSKw1ICpFFoIaeVB2LJG6v00AyduCdg4%2FkTDqp9xrDY5%2FcFsLpwm7v&X-Amz-Signature=4aba845f02b5bad6d5dae5803534985894817750b94fc2c51333bc7bc53d00cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

