---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466257HR2OU%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDBZn%2B6Y4AkaYU3oEYVt8eEv30LDPfg8QJVfZWBKqqkVQIhAMG2PWT2rJLZgVGj9y0ZntKW2Y%2Bn%2FOlz2zUAO0KxetvHKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwPZhvN1tpZhfmkDCcq3AOivPze42m1CWuYnRYkzjnpUCvWqIOU02S1R6kuG0KFnEB1UCkL6KeqGMdTv7VlvsRZXli%2FM4277teduio7nRFDL64am9FJILPHDhT9prQcLjaxw8FprH75JLQXQ9scQJzC8%2F3hDcPAlb9WhjRxkd8RVV%2ByO2UCrsiUyVsl60%2BrNDECtALVlESptvpBbnXHyXAUmo%2FzSkQkO66hq3yOYoZDtEBZ7YqbH4gYaJmOHAoxtb8RyExj2MgSjS1v6k%2FuqzQAXttfoTdaHnbp4Q8SUGZYxAWiGhtGoedhjaIRdOsJo8t2LR8LS9r5LF4g5xqbbZHhTKhR%2BQoKu0cT4Yn%2F6LoURKbXYDIC8uszV%2FdFgIF86OmtOau9B4dR9525AAXfqs6oD1LtOIDyzqCLHW542aI2GM1hXXgqNGWDkWuUiCNlyeNBl6CyiDcCvDtWDP%2BSK%2BnIYqttP617svX6W1X529MWXwFIutO3Ou320kWLRHAoBl8%2FqqZkWpXOQ0x%2F%2FatkRZ%2BjbZdOrhlc1bHz0Bo0zxcBbQ9AcB9ipzf750jARYcso74y0iyx4uGT5DTCTp14EWF6Gjm3ZxrOrWOulIEXCUZOIpzq54WiAWBQWLOElScYTUfSF7yB6kERlw37mjD8s%2FvIBjqkAZ7%2FJGPiGurSMgYJZQi04ftvAARrbc9uoL%2B%2BH6R3v0RMfTczAPso42CIjPXM9uYCGNiyBOS4Hjol2JUBrcYI%2BbgKm2XfODuWfubxBgQx31hetacU1V5L8QeCWDWVMDUHtQxUJ9fD4%2BZPUiS5ZmK4DRHVJRii8Ao8wfTWsTaPZLDwBJQx2BCVym8Fe1AEGPVr7nMpeN%2FfurWDFaf0No2BzbrdwLLA&X-Amz-Signature=2d0db965d6e4411d869680f3154cf7ef1df093d5b31eb0d0f6ee461f5e892445&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

