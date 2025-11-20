---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664JX2H7HO%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAdTTRSDqzavtnuKYm%2FKmAdfw6ul2ayrGcoo2XLUeD%2FbAiAVTpS1jUE4kTp4qoqev0MxG5jsZ8xewHOGKO1chLzisCqIBAjq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwRMy26Zw7%2FJn6rE9KtwDjJmCvqpjxSB4ngzYHPz6imY7kAeH9%2BtAOygA1G%2F4kyec5XFG%2F5Y8RfhbtpW%2BTsZxPYNHodSaQUDKq003Uh3WtKK40oizDReTLA7yaXF7wqg8k6VZ68psX95h6jHvRSYp%2Bbqd%2Fe%2FZ%2B4qFujS2O8n%2F3DgWlcK44rMYq8dRBgK0IIx6FdeTJ0kQhimxvngV5O4VHDaeiJ7hTwNKK%2F8HaoD0ZIUZHnjHhQySwjyfUgdPWQQfgFTmMLXjcTExJc%2B%2BkIcmDcVZS0x5reWp1%2FlpO5qIlBGG2%2FSxyOGvUVrPc2FxMLS5MUvzrJt2b2awV3760X%2F8fHORMWw5jKepLptP62MJsWlCMWsM5TErXq4qCqwuuP%2BAYkDvrcYJ6TbmCwbpqybS3tY0g6Q0KuwAus1iaZLIpuOeY4rEiIyBbWXlToFdxAWS51uYAPN5oJj6Zklh7zTdpzcVCIoccR8oob2066%2BbdJENowaLS6Yh%2FHCC9GV4exdovqkACY%2B3UirCx4n8YmueOYcuKhO0qrCWkyXR1oGRPjwa8PrXm0eNwS939I3R2mdo9ivFMYQRHIRB69UKiM4D2U%2BkhuKAGAYhCsmM8e5CiW2WV4VxACALg%2BHBV0Y9CNWovIy5H1MZhnq%2Fu0owxNX5yAY6pgFcu5WLi3XqzZpCwnI7vnRrMXfPVFffUBJ8NrF7UHWqzP4dHVI0XOaSvnUWjM0r6ry5XrxylirnQXPetAOziDSqqO4KduI5Y%2FkznTo1pyGp8ziOel0V7ZZEe4xpDbTI8FWTe1La%2FoevrGTQEOB9US9M4QwXnucx245wD3Dw%2F4Z1ptny6noSNsM4olpsZLyj7k5kS89xuRCwTDWFKkWFEhYNr4QeQyB7&X-Amz-Signature=239cc0d81870d01d3701d77f72067e394ba3c5ae5831c16aea2f206d7b8260c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

