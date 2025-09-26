---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356YCOEN%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T070041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRv3p9xS9vK8rhsc%2FHF0wIypaot5b2qNJ%2FEKIgCIxL2gIhALC4ZFu3RGEuCZGEc9M%2BznxrqjIaYwySrxOx%2BHtP3e5JKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRl1bUOjWydTJSAtMq3ANo4xlhZXO4iSlde84fHM9%2B5ZeUk1uESB3YukFcPYpJMgmLmpK2GCNTRSApK3zGp%2FD322va41MfrcrLY9vXWr9QvR3SnYQz1dZKHKe272ZAFd9F%2FDAfnBARhc3K08qQXygZ1AzlzUQ7L%2FjFMP564XILb9FsLooFpboWtbCrWcG5j4JpVweFNgi8nrvHvjMESNFk4GdHNZsnrZxNGug9fbvgrF4kX0%2BDGlsHTnc0Lecfe2aTr6lpNNJKxPcLfwb9ke5DgH8Mkiw6%2B%2B6KG92%2FGHsOwUKxvZsSGneql61j6nmxUEXzxdQO86v%2BUC3R7XlGcjlD%2BMs5h6%2BZickzD5trGyJxSHHwiDrrhrbGg%2FsnYLtpRzd5q36%2Fq1DkaNIyCvAV9MatHV6H6rbIFPpQHCvQcG7WTId5dQcJMj3kOYgYfgyuw70AtJdJax4WZbnPIVMdpWKQFBduSRVLkUzKknu3Fd5mCMAj8exd0LRZmoue%2BLCxD%2BsqyAHdnIGeqmHx1LIfTvqQnSXHADYDpBuAzORgJIZVVT6T02N7Zd8f1PDTe8cjLRb8loY1M0T04gfgrIG2vQb0%2Fdh5eeITH2uAg9IZxv4OLecVI8CjFl6FuhVnsz28%2B2JkjOoLRqt0t6KmsjDB2djGBjqkAT0zISnIJofSztL0W3qsH1PpUHMpRVqs8KC8cSTLJjxxpnN1oFUVqL2HDtWKcaZXZDOdA%2Fc8UJyPkPbvP0cviTP6yC1Stoftxeq1H%2B9JhPJdhJCB2v%2FdC5PlM8k5QpPDR0Kg%2FKHI7cV%2FD3x6HTh4lw5KP1TZorDpwgHNvyv0PQui5FH%2BT99KJwThVHgq817ZZW21gg%2BdB%2FNLb8Ho7Hl926tMjK65&X-Amz-Signature=ee072cb0b1d59f175eeef385c4a35d7ef892898fb1b4a7a6a02b18c9f26626ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

