---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCVEHHJ3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVupvX47xv7A4x9zkHTDi%2FRkJgc9bI2nV2IgnIVUVp%2BAiEArWXRJLkRNz3lTgQOQpaTHHNAW3J%2B4w%2BQfixqfKXnzNMq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDMRF2%2Bxp7O4yljYgiyrcAzFHo1u9hc1MDmf9B8TBHxiHmAX0oTF2aGfSRHf8R7oZPub0swVliuv5ioCdAhrP%2Bz8cl7OiyT2hY%2B%2FRs3Fx1AMpbNhEl8SMoLIISBAAhLek4gfnRhzDi3vgHdj3scFRLKG8w7fXBfUpQKoJG4V2nUxRVvBFWFkfawiLVuqKHsRN3YHLdmXTN9vbxIB93TQHiJYit4tCtuw8q0mPVcFM35cPC7sUIiJRkPTTCoWa7s3NrLHZb8OpgGH4jom2FFt0NJFBwAdsFNj8yzF1lA2APpGEsd7tY%2FoxD93mTzMEHdUhNHC87ulwABDRaP8uE4pGqRO8rrOfxPB3dKeiocN3pTZxiJC6yoGx31KJ9%2FNtQc%2BUpEHOCwEGy%2BlhdE459sOWQPHpOdqZ7tI6hFP%2FJqGTQ%2FAyGXxDy7%2Fr6dySOaNPzU7Y7ecd%2FPxuBtIqxpZCYUyHzAYO8blxwtcSIAtnNa5PIX6lka1uLzPOq5532om5YiCkphKwKDBNV%2FMJw6xUIMInGMVW4YULHjpvHkbbuuS8mMXgPrkq4cmQK6km0mQ7ash6iH%2F6%2BQUdzwA4mtNLTXGdBkfuVpFgV78THKgVPmuJYAcLIi7UlD2j2sISnBJX%2FktGeeRx11PAlu1IsSrBMMeH08YGOqUB941VmpyoolnihT2SWFnWxNzOLy4yUfHd2o%2BnrjFRl%2F6QJ9YAm6EGDClb5tXOIKkxhlQiN5sM7r8uIqBnidaKWZXez5JbtR70FXGp%2Fh1Tyx9z4TGZlrzVt5viHADH6DWGY7jXxEcudegs50M3iRrEfisDriy%2BcXMCmnXpjqn6sGX4Rsa3X3EOwwUCuZdj4Gm6rmlKFeHKOZg20aUro%2BMxpfRJAqdr&X-Amz-Signature=ee2d078529c41bf88018bee2fb9e5c8aa2a14ba1eb009ac41248bf48074f6907&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

