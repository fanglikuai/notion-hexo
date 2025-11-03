---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5BAI7UB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW239Wj97oH0RVlV7hHZg75VKLVCcnOIQOKMvnCPAwIQIhALFEu%2B4VOCICLRZvE7svFwxgX5ei2TrDKMC%2Fis3iRKzIKv8DCGIQABoMNjM3NDIzMTgzODA1IgzxWMWDQtI1Dj%2BGVKYq3AM5WS0K0Hfh8J372dbQgGAc6FuaGDh7wMnEJg34pwVLh0khHHq7O%2F8PCHVo7RMHHk462DcKg%2BKKTlI21QcevMiqbTnH9L4rEd%2BuajF9GY%2FZcOcqmhKGk48NznjopqAXT5TEb%2FxIhnbilQXsJnIjZJBgxRRjShCFjDGdcXaWk0QE1V1gTtZ%2Bx644sPs6DHvRPqtG3BEjOhOYHOxlzPdGXvdA1fcA7Z7xr6gU30ka2kaIOpk%2F1Di5HryxCdA1CUN96V2GQ4kwHNj9%2BP2x6PfWYw0hAFd%2FnaDkM6fxIAgH74K8PKyyY9YTsO%2BeBea9qJnVAEmrcLVTokKqS22CEsRogcRqO3G94FLtAoc5na5HAjw%2FDKJMzROqNFEuf%2BhgOp2TpWBaNIdbENFg%2FnvloyB6yNNYiftsIFxxEyflpA0dKUK8SvcDyHcl%2BEf3XY8DLlfmkuUhv5pXYemmP7gnrd35sFxXy3pm6BZfaFLDtRBXGCHdOuGIGuuLXHtfgBOEl1tWsTHs0lHfKZB6FYBLbjqwhvhtzH7pXKC5TiXf5FkbDK1t1XsxUkE%2BbM21wp%2BOibuURNjcCOKDYbNDbfMFWavd%2FL31q8pRwWp8a1Mt%2Fr2POcW%2FZd%2Fqf%2BsBPPWKJ4ZTnDDTs6PIBjqkAUPqFXhdLj3%2F1gDJvv0yzbfX2l1DpJy%2BAf4BerF0qNWEV1n30U9dgIWKQfY1sRGj7mDqao2CKLf6cKhT5YDTLQus1MPXpGMQveu8YzqiXjnM7JzArhC4yYTqcvsUN4zWB3gU%2BjI5uNriQ84wDlK0awYP9fa8xRlZyKQGOTPM7pdES1jXSbc0trdTWyioognZdZ1cscqlWNUQN7%2Fba%2F7sZ93SsGNo&X-Amz-Signature=fab0ae9ed90efb21d9c1b76d5c15975a507c4dd7bd3b90fd5d31e8194f0ce74a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

