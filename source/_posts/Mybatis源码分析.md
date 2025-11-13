---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3Q2BTV3%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEfpmUrWYN2R59Erlecox7j0I%2BvnOnVB8J6cIc96a%2FOwAiEAp5yK9%2Bvi4s76CBgUCrGPUDRlJf68ZaQbmFBvdJLXqdwq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDHNMU5G10SUUzdDQgircA26KIPljov3DUnzubYqakhdcDqdf0ondzD%2FkYVswBA9pWt1Lh8W3PSZr6LqUbiOXmMFvoOVXnbB8iJaMrIckhMPcZlRHxrAXhXlaoWzJ%2FsA5cc0d2aswPc7ZvWPP0UfziNUdZE2E%2Fe7AUW9TAJxdCHK3siChVUvJ%2FKYWBAVymp32Izqz2ZjP3xt043uAZbNqe6m0jkkWLhNeozwlAkYJ81AdCiunWHIdzTiiu%2F%2BsBi6OwZLqMDoJ14dm8pJ5ZOI6cafZroCUKJT7%2BHqL4SoxZDAwFf%2B5pJT8Xa83IrAByjame9Y2s7YPdAf66RUdmInXUyW7LUfs22VPU0%2FLfvaDOnkTI%2BmYcEx4BwJFSA219snaShdDkLtmxiLwHPwlhv4uanE66CV16altqNYoIFcaEX4IT4JD3iBj8T8Ip9DXe9tV6AMw7UHuZWEFl5IEVshiz2Y4clDXRAyYSiM3ZVbuN6uw8fjCpf1aZBLco3xUytNz4e5BZkDcYH%2F%2FB9F871S8fecEbF4CQRm3elUFX6BqcZnszxrW8Ct1MqjV3Ko2AD61fx6FCrIMxAHpxzVkop9fBrXHmK%2BBCOacbP8V2V0UfdgHLO1ta0Lc%2BUSz8z1IApQxKRgrA9JberIg7qr9MJOy2cgGOqUBY9KQXkvNfz%2F4T54rYiJd6M9d0DSF4K%2BpeDjfjg2IPYwQftILAaOjDHL3SwsAA%2Bz0WNOx%2BPOpIt4oU5sjoUXZyb2fVraMBwpFFQNKmNRbPFeIvAiyuDE3JKcECEESq1RqUmd5LKcMHHbZpFquFl80D0sChA9YnhGfRFXS%2B44uqPK04A8%2FECPKptnJu9IiQ3H4vC9wb%2FI3GP%2F09gnRmOTg6yKLfnUg&X-Amz-Signature=73c08bfde646539d0097588916628c9503e898fbee61515547f462854ab97d70&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

