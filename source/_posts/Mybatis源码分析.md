---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z74PLTBF%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDzKdxaC0ZuLBsKltGi61zzCyZ7s681uEjIJT0uKHSrAiEA6ijtly%2Bu8L9noCXGFU68Uf9dU0LTCXQUhfX%2B0qOyvz4qiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHMjTv8AFgyivebSACrcA3uWNjTzBMCC0OLPssS%2FiKagyBQs13crB9qW0RAK%2BH850QMnSVFetCGGOlD%2FYUbGsHw6sTROL4CHi2IH%2BIYE4R7xpfsUvAv7h9LT89tZBrfQ6aVUvL4vT40fLnvx4r69p9bNZXh8X%2FZJy5YCMpoTQ6wSNngl4SJoCF%2FuhDS6B%2B2pjDgK2nUY%2FS6wersFFdAabZ%2FQ%2FkcNIlvz%2BOfVxUlNeCLMUHMYOejJ4OdFH4FGq8DdQqszwVtGbJ75GJlM0hSwK3%2BnFd%2BICfrZWtROvOVVbtwYhS1xerrFbBrNZbHbETDgLnJHVUpvPfl8u5tctHTVEw505ZPgHT5Y%2FMTG8wLxorTkchi%2F1T6%2F2ufFQyAUw59k6xhueZFiEDBnWuVbMy8SgQrCFH6lcVjghCKka2oJ0f%2BIag7jRvuk61QSrpMh9%2FP8NyGyYvE3UkfnqC%2BrwOG6ZkQGaL%2FEb3LpvSJ3R8uTgnGemVu6Krv6yDnpoHBwbmzZC3ik8r5jwV6I%2BhmVYSdew2BdW55n4quZitNJyqlJigLgfU0o54ziG7Ixfdyqik2g%2BLeaGpCRfuQL2ZjVlSbGLxdC7HMeAQqL%2FmNo%2FpdQeJHaA5TO4hWdUzsBWUP5tc%2BqdjLMtY2laoYynFOkMJXFuMgGOqUBmb0U3l1fD4M80X7Wa9tiaQgZS%2Btl30x3YunZ0Vp8JFjx5qXufSjWxacx2FJ22%2FNMYCmZscvoq2rU1z37Ds%2Fd1ky%2BcZqHhSJp16PjwZlZuly6o8UXMxVohqLnTc3rJDeAvmJ%2FLuV4%2Bdy%2FjRiyFNECia8Jn0%2BT5amiI6kyfwIZtJ3IMz0kLIFuyVbs4WOKMlcF3UTzI7DZM56cYIgIt%2FuXQGrSbfRv&X-Amz-Signature=ee5a0bc929295f92c459853d96ea4eaa668088e89d2b0fa5806d6aae03e7d667&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

