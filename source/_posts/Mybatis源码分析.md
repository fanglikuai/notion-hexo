---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6I3GURP%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIERwH779b58ErCYvauTKFqW8dSQL0ZmL09gZlfhINrnUAiEAnT%2Bt%2BlaIuaC6czNbRKpAqr2bk15goijsrR%2FBsTdh6jQqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkN8%2FXs37OORz1tlSrcA5dEXsx%2BhmFKx7bX9H3E4ugjAKPCD2IQ96PTkCpIXF53mPIerANPQEaRzizGYgMNh1J72ojJLWf%2F7tYVakcy0BtOU6KSJb5WaZcG9jL%2BD%2FxgYD9W4vu6D22T1yenmlKY7%2FEt4rRatamojO11F9xQ%2B9P9m6JmNND81PBOPtnzV4CJmOLb1LEyl0DLACHN4I49yjtCn%2B4DnPUphyErSh7ulFflJMs7ewWZ0UBY1NlIIFlbdqiXwlkGrtKLzN9WHXtCh8Gp5e5SKbI2qhNFa%2BZaGcuiJG%2FMixFspy0jcTIywhu3RvoMtbUNA1xU9qCb57Fas3CivJoEELx067klc5xXwzIu3Od0FOffGmneul5YwPHu3tFNZyUtIgCwugpNy6mBk27%2FKhw2PBC6uqUCeCxsgyakLsGdcKWdxFz1Gf%2BrzCOCHkXbDBO9oVjtbGmMvUAyUms0ybyWrYQAUCyI4ZbM638FK5m34htjQNk8YIsGTK9o5ZxA2M7paupeLEogW2A5BTIq36cfG7jKkMvxdwrx6kjDznLqoMB44wALiV4ucl5mXj%2BHQnSWIbYK96vbngEi8sUrAqWanmNzcnxslYnTZqj0cj6%2BUXDAXr1Pa%2F%2FjBgHGH5zFcopb28S4qWrjMPzsl8cGOqUBCBf0EJIM%2FyTxmciZLuPEGGmQdCSVEHdGictw8JjUrO4SMDkGBeJMeIhYfDGXSl9JDfhNfws37kZ5v%2FXYwouww%2FwWOsutf7dBWRm%2FPkqpoXxqNe%2B7%2FSJdJdklZV1Y4BouiXHKlwMwLhO4JydFCl%2FEHfUThXFuOhc1CGUkPrm0W82VZSxtu2upGg1okyFVg2F1gEe1No7fcP2wIJ0bBC%2F5YwI6sLjr&X-Amz-Signature=a319e97062b5eb7801795896d5131dbdc74757928fc1bbdb1a1af85de8043292&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

