---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PV2Q7LM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCGfcabprnaXWTE8WVMiS8BNnQkSXJSXXkdQ4pRCaE7LwIhAMnMgY2sujYoGNQV2nK8LXJVfPpDK1AukzCHIF%2B6vIZ%2FKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwaW1k7OKDRdGM7Bv0q3ANqdBuaGGjJ7iyNJNnzxaD2y%2B3j3J6%2FMXtfTzWLpxlVItaEth%2Fi7GYVoECebd3iAwYrRwAxzf7kbeGfgFmlcGX4UIDrLgR5M17UexnG2O19jlb7oJLAvkmznHD7V311EgCQMtU9eRw3LwOOZ6qCz5lf2aWFxzXAnIe6UbDZHYITtMrfrjHOrIiBwuxPYal18f4Y5VJyrJDbAc09d4lB%2B1IWaUbMlbNY8qcN0uI5DYHslzX%2BsL17mD82R33Hl80MCGJljeUJ5Rh2%2B93fse6rGBPHiJqOA2av08dFdYyzqVbbi5wMjnSviGg2uX%2BInlf0UEqofDnxJXqpelqI2sIIpfaamD5wLnXdntxWsauGw%2BD2pmntKtQUOd%2FhWkY3PGDOiIr6S4QjnMXQXhKT6FhCBvoV2yf0io7M96Cn8w1zSQ2sqbGBN8ymtQK64TvuskpOe87kE49%2B10NefuJUYORXjQgnZITcfcg47BPkbeL26qN4JKsVB%2Bf0e%2FQrf8WApcpG3LKa9bLBMvQ2PXBRKsc4h2zXzArVuUct09It5kp4iPcrll2PTiDS9VGfiQ3MFxvXdNIzE7WVfWO4kfTlvYXdtaB%2BDLPtMLqDTgyo7AulgsTbKoh9Qm9qDE7zZ7mpNTD%2BmeLGBjqkAURZim0qBYh5l1LsK3GVG%2B76X3z%2Fgay9hsjhWOOuscLMCtLI3eeMCSDJOaREz6xFDV%2FIfNcuJHSXm36M2cojfgJ%2FFqsHDucHKyfgdqf51DJsk9xzGQnE%2FBQm1b167K49I2tCgaXtYmItue7cqDIWwN%2Bh5xBTOwf8UTyLkzBAshYt5oxtX1lBCa61m48FKG6RZlpqH1lGyqQV%2BdWe0zWhaCTDxXja&X-Amz-Signature=696deac5340c4a1e6a5451822771299ab6316e03ed38de5fccaa6d302fa922c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

