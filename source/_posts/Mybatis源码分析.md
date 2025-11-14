---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQXOBBEP%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEbzzkzRmZ6B7JBZNRYFZvpgYAIG%2FMVWfpLGGgO0V7wfAiAKoLb7FSQBmGH54CWCWLXHerdMKoZUJjjBv9tFD5ejVSr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMWtjThEPcLznwdC0JKtwDB8VmQ0AO%2F3NkHId4aEDii0aYKXuqMLcpbmigduWwk5SeK2Yd22zNOUbQV9FvlgVPnzU8l%2BpaaKjcLo3L5J4vNDCB6Qrv5Gm0j0FYqfUbKd3vHlNH3H5QRd3Vh7DrjLLzeA0LPMJwy8CJeIYtfx1WSiWOkwpyvvbRkoWLXt27q9F7hJKIIEk8rWhKaLHhVtvzgVQRrfPoU0KSQWLFlYZD33VcqnnWjQ9Lg%2BJaW9tVzLZsr2cZNjP8WCc72SXyjytruLu0wVkbzAPHeeBOIDa4mfSl4FrR6W%2B%2BIT0iWDWkv8Ug2zBye5fxKsW%2BsL8dW%2F5XP%2BuH%2FPwXWbbUK%2FUOeD6yFi7G9B%2BHCV7cOf%2FlhKHCxbbjbEaiH06SV31PQNzfTWi6Mcdpiv16nAE%2BBhrEPo%2BKVxDpFgw1gqQaKh7vZZ0xVWotdyot3WMnhbG8rTUr33QKUiG1HsTfG2UC1EUdKyj0%2BtXSTIXhzXbTRSNM%2B9Q%2FSBna8RjGbEASbSFPNafVQXf1b6veyib6AbA6UODUR%2BX43qT4poEXlVu6NSKto43zg3fWi7SOPGThmonmhd%2FlSmv6Z5%2FX%2F5scKUDoPTFw9HkQseUuUElnG4ja%2FhLEaR1R%2B3SQCy%2FAoNX5Vz1gkXMw4JHeyAY6pgFGXoLBEjNt%2BmThY%2BzTW3YRB1hvBUL4A%2F6iJEL5AifBKEkOIASTOhas0uQXJ3Rz8pGumVE8eRjahSxf4NJweuiL5OD6BwR6JGI%2FWg7ytWq8ARxLjN8RwKSCTK2Gio75iNHi5D6OZwq%2FyKjZHASSxZinwNDJcTMz5Gh0ytNSYgm1Umo2vuw%2Bo85Ju0QUtyeHVrqHryIp%2Bh318ulVNEqPq77HJYty6rzJ&X-Amz-Signature=8531af4bc8f0e384a0f4805ec1d0ac89e2672f9e8588c39649e6ee6fed7efc3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

