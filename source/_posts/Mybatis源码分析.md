---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAOSTC5Q%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQC0PoOTundYwI5qc0d%2B1zBcFH0ci4XpMnpmceV8%2Bbp0iAIgXpbZMYwa%2BLUD0903kOO3BvCZQz0bQZljD3XiqJHEs7MqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2Fj1duEUBTypg1LuircA9U6LlXPz%2B6E8A7edILlvXMwG2AcSoFbEKKWYRhubwhNyBFBx%2BndWTATRARZ0zHHv5lkm1OJbRK%2FhK9H9pgB5aB0EfjGYyG%2FOOdzq8TOK55gqpyusadFwlSz0EGtymA5rIuvEMKrL77Vr%2BhtG9RISvG6bm6DUuaNUBezS142SbVmUmKUyyRfozUeJ4ObyuRI%2FW%2BIw8eg%2Fgu1GzQKgC5yDYamwJstDzvfSAP8ji9DrFjw5mJU6T1lYCvqpjWCYjPNCWD80icq7KpAm7OI%2BtbzM6RJstAN%2FHz8Ua6RB%2F4FeKPRvPjo0ErxsJweFAFvFDTOwHHJM3n7jE8EylE41VnRwZLaNSrfUClyp%2B%2B%2BZIZ0JDlyUyMCTWqr%2F6oVtnhmiPfblfIfsXNDjBszgTsCoa83xnWe9MEzeufQbPdmtsErq8RNr3dz4Nf58aoDCHT0u6d1e1pEtmWofGcLAt3tRt80dyRY1Y5Rh3kQBqp%2FN%2BaxKLDvbpfO0KNfQXZYIqZEF64aY59OdrYnHUh7oMIZxijc5CiIjKcYFaHRwnIt4HmzdRBpDcMrlUtW0ZtRXFzkvSkRs412UPlGcIU0O9KAMQxmXIjWpz9ZBR4zxJJ69nnxfZbMHF0m8aTszR4P%2FEAlMJGApscGOqUBgyV4UTdAPDys8NIedTDaeOzGeCMUVnPzjQ5HRZQvnTTeaHn9IqXWRHxZE7ICkOo4CWiQOgjt9Afp7nVxw3dgPUPPXtU8B8qMdyuTFjToBXFSKLcpDA2oiV6zORoTYpgzt65WLdcXCjo%2FJgUAQtdo1KjTCf0080TXAxfDDxVkBg2uIVdemoQcMuIxpprnXqZfvDyB08c5%2BOpLoFRU0b9j1VuwNE8m&X-Amz-Signature=11393bfb7dcd16ab7e8c62a35ea646509f61a26730e4a3b022232c19a212554a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

