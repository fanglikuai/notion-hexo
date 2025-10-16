---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663W6UN4MR%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkwkmukkU3Axafh2b8HPMdj%2Fmx80TC841vSS8BkJ5izwIhAJoqRii7HbUsydNE%2FansFMoJl41TY0E3XXTyuJRTThZLKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDGbZ4D9ISt3KqFWcq3ANB8HZOckrUmrxicAOqJNhjSdiMIeMwghK5oTOjmZKrWCqIXhp8h9ffKBlUgnAOL5730aIJxVSd1IP1mXvrL8sEKIXl1jog3Kj2N4se8vtvzvZA4WrVyg1TrYy%2FPoOGjUJUU%2FyaNxkO2E8Q2sQIGBFQWOjZNnjN7rhTXiK8C4SBU8%2BlXHUxZfOTHFtGaPCEkUM%2BcFwPK8zWsZ4iG1JQmIEW549QbpIDAP96v%2FXI45kxnrk9uC2O37BbiR81p7Cmz5Ks3tHFNEbpG2BZFRrD97cOun0qEm8rARx%2Fk2tuXGlTGHGqnlm2U8KArMv1VWYZ7IO%2BS%2F2NRUux9cYGcf%2FzLAP9zy9TpQzIXyVYX%2FxKnHZVasFp37X7u2gGJMNt7gSNsbCr10opXsDNArMkzfEJLBM%2FgbKff8nJsPCMG525uR4fgNM1KVlMA7Gd7GwVlzjcFxRemleMX7F8LDQVopLEsv6Ene2laXpfH%2F1zVBsE99jodSWccTTnRoHciXrrkcujlYhjcIowTYxXeRe1litU6%2FzS%2BwL5A38Zj3%2FaynH266LIwR4l6x8WmLVyczU8dOuIYhRo7stdiGGWGuCAobTwVTX7eRl2KI508BeWQ0vvfJ2faUyfwNRCOxNj0mg8zjCGjsLHBjqkAdVJgOtt9K5T0wigEmQu9av7u2mvorGNkrscig6UcnQymNa5Qgacz%2BxW7svVq%2FwcYZ%2F6uCJtMwRiLRoR3YD3KddI4mptBBE%2BWSdq5RcbmHzqLrFwlXxv4LbMiIHottsIrb7eaLjzvZrqW0MCUNapDr1lxBrzvb5waYcpNkw%2B5a2Z84yBjeBlzvvZNaKQr0Rps4d6dJ5dLAVdDzAd195oR%2BPLYlf1&X-Amz-Signature=88d55bdcb831e051c6b0560c30c759f51f4517d5a11ce0516f4d9f1e69a80844&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

