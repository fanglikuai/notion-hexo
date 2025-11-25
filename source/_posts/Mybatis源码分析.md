---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI5XFLA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T010049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAqe7Blqkf%2FQFFUqL5p0J7MQNFk3am2P34Fa8lxHJPJNAiBCV1SsRIX%2F2zHnRtc%2BrfSWzaKLMWBT7NCTDNjy0WAwqCr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMFdHZR7p065iy%2BleeKtwDQFp8kLWSZW8IdYsxxTzR6zKpgNfDOngqea%2Fwt7bH9zAwl3Q64Z67TKLI8TL%2BR859D6v9jZmRK1qdKb9mAj1JS0YBi455%2FinfksXlQ0yJVABJ%2BBwY4%2FJTfLNhifEQuyZaBxpYe9bXX9Uk4pNiI2qSTAhL%2BSXRYT3HplPlx7gYlD8NOHM3tNXmioEfU2OeK3G87nkRqFhxPOVKZCc%2BrX8SqrFjKPFEtKhMzVq7W0UNv8tf4fGmHk0xeX4UU2W7n9kPkcx4il9bCaD1MHzxRYp0V2KkpaPJ%2B%2FYTrwv1xu%2BpM59TkMWjvlkDbhUZPO3fvlqJsWQ7qO%2FzOMmKOsYDUiR%2Fbi78Kr3n8UbeYLWn8iluRB5NzvmKfUIzuHAhVev4s30PpOUL%2Fi6nL4RlzjfmGgYxI6y96uDsYwlGRfo3QG4EIXOJWUMSMnYmCfjYYOWpRsnR0dgV6CWeMbi7m37bx8wxOdW9dCW%2F6aZzjsNrfvU%2BhWJnb41Av6W6tNlPUBM%2FbWdQb347AfQ%2Fz2i6%2BB8kOeiW9atMGoMxcBhXuROjfhi5hOP7c5x4lJ%2FiqylwcG4XsfZOHcmHY%2F%2FGYnKtxurYdGBdu2uSwS4QxvdOLT6UQzV5KzFcfeEOTO0hRmHvzR0whteTyQY6pgE9mEFC3N5xAhaa0P0dpLLXJ76%2Bf1DginoOfTQS29kDAHHw45QaRFFfHF61shWwyKhFkYI9tYW2As67Dbto9NR5iIyab9wQL81TnjG9QM9q4DILyEizuHNTaRIw%2BZMfI6JQBRTpMtWxz1gGqYFULnR%2BSClVyb%2B9B0lLNywFkkqhwS5qWv7PadmzhqMpyIDoEITScVrqR%2FpXNaIMMiHZRK8kSLXI2xh6&X-Amz-Signature=9596693a74e32b5e7dc6eeb441991d9342a1c2ce00fdaaf5d908edc65d38c3b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

