---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZPDMDJN%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T090055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQChzKR3YWhJ37EJzg9aLLXf8RECQw1khLdSKjeEoWpnEgIgeX8u1XzKfH4SE2YbLozn2aRvLluD69lbDkPY6qcpBcYqiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABHndnKmizN%2B50uECrcAzuIVgT8%2BiCjw%2FUd1Hd%2FiQVL03Regs6aw6hLCeCzA4fgp00B0vuXMfCOguGucyCWNGPD%2FPdcanNmYfUvskkbMy%2FwV%2F3k6k9HzFEI3c11S49i1v5HhK9JRqblECXuI8D%2B%2F5ezPVp%2FFBFHEVgg9EURC%2FIyxoQayhruinRRZAPu9f1GtDt3%2Bu%2FvrBPHSFSn%2F31WPinng69y6FsGveC5fOVN5QvB8U88PUcTCO%2Fv27UpgOt8IRvuaJqOQVAPHdAYXjUh7cd6ehngjbsTxTIUdvBWQiH7l4swmdZYzPFhvSbt%2FErHF7TOqPUaWHDQ7UuRbwPIjVv8pPECm5I0xGbw5gZg1Kj2eYsFfi2T9SrbZo9414uppxTnzVLPgrf8q2dW13cL6nRMzdLX0LcQ6T%2B8n0l7r8oau4En3g8N%2F9AQAZlQjebTPX6976rIkfRWwkYfN0YG11OhwNRX8Sxvt%2BxdQFB5W4ckkFCn%2FyzHLPlYuG4bPyw0kn%2B%2FilRaeNlFpRR7IbV2sniOHQX4v8AdRAAha8Cb6dmQuxmxmG%2BIAdQkUs20ad3Are1mZ7HUKP72sRpVb8QD8m2kIpTlja0fMx5gZo4Tpo%2FcqMdmA2KIAwlV%2FNapx7r2KjcEWQkM29NhuRuiMI%2BTk8cGOqUBvXSrsP%2FQpZhYJE3GOVUy%2BCmcRg9fLxGONWMUGUvYPk%2FEi6T2Ds1X%2FSZLoVhIxiYvXGTTfukNZUM82RvkleluUELVjBDimSoBYnntvEx0dRHHJDyTPDrTYhsx6wVkpRuNuEOdJYUy4DjC0baoo5%2BfiHLntXRPVkn39SYiytnJx0TXcdzUC6NvFJ5dOrCdfv8FTY3Tipt8IBMx4QsOCp6gdxr%2Fw6Sv&X-Amz-Signature=3122915a16b9624f58ab0b56750d4a0d38c835f1e387e609a38e942d094bf13a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

