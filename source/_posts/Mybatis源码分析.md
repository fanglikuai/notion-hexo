---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664N5SYXG5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T030054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCICZkHplRQ%2FnWZNYJK8pcldwz08fmAuW2goPTP%2Ffy8AgSAiBTKfS1An5IUuZB28sLW5BCeWXgxQcvLpaiLFVFRlwCiyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMtG1jxBe4bsgdXp9pKtwD9Gee1%2BbrHGquxEy9mTpQgIBytyz14nOc6Rcnzntj8T7qKOPL3Iqlo2kDmB4rLvUG%2BaFuYm%2BzANvhpC1Pz74oZzyOM0y%2Bj%2BqaM%2F98H4vBr0SaMmoZauJukbQEUiStY25hqHP4syGfgBm2woduCCMIY5v6%2BGK2ubqsodrSymC2RZ4bGk0GNXdtlMwDDUAYiUzTfRCoC%2F1r%2FHnugg9c6uuPVCVqKhj6p6ycJ6iHHss%2FvO1ARo8OYFw77Oy3HcZ%2FVEV5JocYxl1QP3PegJMhqtnbnymoXsc442pDtQNC6Q4i4Oh54%2Fnv0wOKCwiKaAYMmXtoXAnc8MijMT6CrZss6Pqt4j47ydkxfNFkJOuXX7nuRzyc%2FQlJYDP1oE2eVppXMI0gn3SX6M0lHPEawRrZCq2GH9QDKmFPwJDVspxXtUZOGLKzFsO2XUwWYIhx3kf%2Fs5%2BnouNIp51VE7vwdO%2FVvDXMFfYbNbdS6KMyVm9do%2FN%2Bhj27OA756hBi86AndzKepb2pLAjRmw7YinX7V5pgoYRR8jqmOdypoeufAGsJjvG4w%2BiWXKaqU2f3wOtgqF%2F8O5pSOKxQ2%2BC%2BGozD87qSpXhx2eXST%2Ba%2B4EYzF%2BD3tbEfLlUqkckNOlQvtuD767YwzujgxwY6pgFGLOpaUVXnOSjHmgXD2on5vKx9RP4JSVxa406Fux2aE3RlOl%2FGptm7xecg1QA%2FlYYEbw2QE4zV4KCqGngpuic3VDtvEK8KrO%2BmIqNlJt6zVBPEnNopjFv%2Bu2SuPIKZVa%2BI%2BonGdLfcN9Mrorhil%2B4D2IQ1HRZhmje%2FXJ3TDxiP%2B%2Bu1GB6nht3B%2F9njF97E%2Bugv6lToWvW1kT6Dub2ZSkSIkTPuUuTv&X-Amz-Signature=6714410766ee4bcbbef9ce8e736847011863a16b238e0b22f46dcb61ff5736c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

