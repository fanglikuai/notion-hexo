---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HPCDZKG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQCy55CIC%2BklDTmtv57rWgAQqGKgKewKHdA2vf5S4Ri0JQIhAJEEDnUSl1Vv7Qkt%2BXH5TWDc%2FhVgXB6Z4DOiKZvVETTsKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwvzTlH%2FvH9nmdXAMMq3AM%2FDay58yeKU2jgQvClqkB7Cz%2FaULetnQG9dYQ9NK01msnWzI1XYZApUxF19ROux8NH7TSiFc1QYG8MiAD1cXnzHEgPy0AHHYb%2FlnE5E%2BUu1ImZDKx9vPe6zxc36Ln%2BfxczzJrJfvevVrB3UstJtayTug7IyuQrYpG56bAIWLQflp%2FXeOLiNeSpLz1EtTTm9I8WL%2FjCYeTtyU1Zog7adQJs1ZmjMYRg8vxmtUbxxE4UzK3HgiAe3D4VNDkhTcK%2BL9OLchNIXxpsp3Ygwaz95HLiSXNztGqzkjLu318I4aalVscyVtVZCb%2FhSHzSc6Wy6j1Wq1YiNjOylJw%2FEpkvvIqTW%2FBfKq0DGUGL9LWz5qO8fcPoqWLUFp4ElA%2FEdhyTgTTGFxWYL4YF1bXt%2F1vHefvu6WgV9WeZ2GaErO3Y4i4CWfvJHaovSwEtxDaI41Qxi5sEBf8ptTjzeR4j6rp%2FVOpt2h3T6L2tIk5s%2FVfo%2F78O4L9fio3Hx3NS4d3i8AIP48VXkXBzFBvIFfz%2FrN%2BkosuwhAtVcCGaVNzeACsNDydL5o2mOUwi6mUbrEf4G6LRhMKqPtiruf2DlJBb8N4Eh2ay%2BEPOr0ElFTmKqcmungbGU5wa94hG25K4kHfNODCYyrjGBjqkAXCijC1Qg%2FoNzGLaOzvYqC%2FIKyquvvBBxEDW4maOa9FJF%2Fzh8TxKxvY5geVpIzRo1lK6PIhHftMP7g0MCXzuSWuuHRkzXMP7tuUJRF0oQ9X%2FIdnVd6QByhOmWgAPMwpOUObTOC72idMjwmhcHoH5eiIvG8FvAmMXKYI5DIiVQajWF6yGTmxfHiszQpcOUIjLL6eDoOjbRKU9eHZNO2s9Pa12xDPa&X-Amz-Signature=05d41855181a0fbe3f7e12c6da51b244d91b5f24cd8790acbb7100aedbf21203&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

