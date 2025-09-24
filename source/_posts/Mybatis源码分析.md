---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWPNB4J%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUt9UNkmlyNKeC3evLyJdtd9LKVhgaRPW255ni%2B%2B6xEwIgLPJzQ8sLzp0cKdWaIECl%2F%2BdBTN%2Fvr1v%2BxWYL0jSk8%2B4q%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDII8K5N%2FQmcDqyJ%2F%2BSrcA59GxKsMVisWpNUJmnTa2fyWa3PbY5JVeZ%2B%2B9w3FUCCV0rZCyCGJ0ea9KAm8agpAiQCOkHwWKoKWtPf8h7mfgMZEagQUdkWUzTVrO06QdAsi6lOwO%2FSXO19eQn%2BpWI95V8cHx%2B1i3%2Fyb%2F4CXH0lyH8QaJiS404EUEGyaFHU82d%2FLquc1pCan414SBWV0tpBQqt7CKfxR%2Bh5db91ejXs%2B7kckifSuUHmmSm4qQy%2FiwMHc3nxK3dZCRGLHJYdLWjg1QqJMlts080m3RCxFsjGQ9MX97vlAOjVZhsRgS%2FsdcXycy3qr44bfyofasi5j7ztnGgjHfpWd6NxPhytBXqSrmPXxfsl06myNB%2FpwqxYKEpAsi5tQX5NFyndT%2F0%2BZaiwoGil1OoempS%2F%2FalFjEJ73Wm6R8hvOr384D%2F8Ps9SmYlSkDCv2N7TE2DxPj5epIrDESUAHOSdUK8kiGbqSXfBBWLQ%2BtkHwdcu0cpfApG0XMfJIgQSRUXc9vBKpA5eA5oUhLbPxsI9U5%2BPD2H9BFfN9RUJgThTw39SVHIj8vrq%2B7x7FCiGF06Eo2bW968BJg7iWayUzeDhoyUUtkxZP2zk3aMfxOv8O73EBoKXvKD6MgjpNM6SgqZWTNA%2FeYKzrMP%2Bd0cYGOqUBMoyQ%2B4Y6YUKZfPGmw4wD9cDjggFchB%2F5SEWtVidSC7moo4me8jUt4FUS%2BJKTBY8nfsSU0ph9r5t0RONh7KuMoVPlIz7ojQkaOfg%2FA%2FttbUH0jW2FbJ2Q6Se42jy8AZqfYvPvCofkkTXwJpY55MKGZgIU%2F2mghfFmXdX%2FFDybuVvisDh5A%2B4t7xAQPvEg49Y6RsDiXWPGAk66o8uH26e0f%2FZRDS91&X-Amz-Signature=9ba8aa8344fb92ae04addc75a340e9147b00b8b487fb753454f22fc390792d5b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

