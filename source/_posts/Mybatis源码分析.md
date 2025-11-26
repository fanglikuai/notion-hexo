---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEVMUU7Z%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T030052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpgMfU0SC7zmhsVee7hgAVyj3kJHJxTciErXnGrx7yLgIgHOug1NYeWazbtzSkp7qchamAkU2Pe1wZTCfwUloogZYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDMTrhHGkBnGbRitkCyrcA%2FX%2F942yHV%2Bp5j2YAkObrAr1rbr4s6aZmdvwWQkL%2FzTMIMEjpRpp6tV1oQzeWRlyd9TmxsUDaCqsX0O71g3HqqyJ2SRX2JcBo97F%2FqG0NzcVSeCSkntfrhlzPY8EFGpDEWpMAOkK98hJIiPKQ9ZshpdBc5UVRCwoJDALsRBhZRmQst4zjsOzJ364T%2BcBNtwFrsx8nfwQvr9GTmrIM2rxgbb2OURpso%2Bq5LkiSWIb%2FP0Is%2BZyzgdmBktJPusnwPyYxceEJVSX6DbKxuvj8KDbPzyzaoO1W61aNd5d7WQiBlya666Y1G9vCN9aSX7gV96vdPEG2vhYZ6LtOsmhCBb4hMur1vDgG22V1fHb5m42zcadXa01lwG7xjzg6Igdu2Ql%2FbHCEaL8Ja0Nrb6yjqkVZ0OWmNHOikMrvpXn7agO%2B6rlkB78IknwzzKeCyNA%2Ble3D%2BvkWi9l2BzYqaWY6%2Fd9XaF9Eg8rygFDhH8ldPdC2p3Ruzh6rj7f7dQrNzSyHGsreiEbAkbxlaVK6vlzNv5ktMBWQ9%2Fgmef5%2FqVS7X%2B65v98Hs1xOYjmUaq%2B4Ntba0uqMNolpvDbseeou1WmjVWxBt2DY4FQx%2FI%2FsUq%2BQtF0BqJ6xQqyxVZlojJYI%2FkQMOjImckGOqUBuYQq%2FGdFCUgztiYU%2FD363qivioLG4PAv7kAbGj3tIB5Xaph7ODNtuucsbeNNF4N0JzVLE7hz32YHdAuGxIXcOvBqaj%2BEDFYqnHQkC%2BCVAY6tJ%2FyC72bCW1crvAozhKHZO5pnzNsF36dZ3hIExdyKuOHtqpcDXKRkUUYooIjre0Co%2FqryfiCVs18%2BFsVVT7hrq50FcR48Q7UUG0BHDqeuYXKEY%2FG3&X-Amz-Signature=0f4fb9611005ef2a7dbac065b5d3ce49101d71dc45f693b5ab9c838b4621882e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

