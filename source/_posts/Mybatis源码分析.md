---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XNIOGIF%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7sCXU%2F7MzDiR0bMrWs5rJCr57gTFCfDudCVB1Q5zYJQIhANpE0%2BJp0H%2BWcMWeWuU8eybSJszdRgERHSYmOndf4QfYKogECI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzmnKCnRWTJ%2BD%2BRJBkq3ANvaCSh90VqN8GfXNmpMQvsv7dHqESJBy5iGILMsx6SYsvqRXCVQwhFRDVoVHI6jMiVlVPM%2BMR7jRTPGD9STGTF9fAjA5I3sCE6WPxpr8YvN%2FKpHzKrT5Jt6H8vj023i5qHoS%2BePsHt9YDFxQhkG2EYLXvEiRHLxvcxDYirFzA%2F7xuZOKMRAv6zpesuUOB5njwNY0a1E1OwqkVu%2F0y7tS%2FOg4cyDPZ1vMpwrghRTD9%2Bsg9Q0HfgfdDtkfD7o7dIiclspr%2BIjuXUPvjLN266uTa9E8bq4I5QqZ0OcUMOkwoV%2FfozC%2FN7IDv57cuhd%2BcBSEwb7G47Oer%2BqD3JgZGH9Hr7M1kPbuIwThPCf3uM6NcbaGB5%2BcEuLN3XE5IQvBl%2FuKX%2FKJDjIP%2Bw6981RXzPITPMDABkOoPfj0JbLvPQGrDNaxbH3U%2BzF1y1DFnvW9ag32suhnzPbnGpbKtPc1qcnWikyOgIKQJOsm1p5iXcE7OinKlyJLe2QiDjQr%2FCscChrng5%2BETfJwrms39vuaY%2B7pIOQNeYAC%2Fm45oswWT0RFQilo9i%2FqiopK%2Bv%2FblZVUX0zGZMEC%2BjJDkTqS5VNtzFZbEA9uIPCx71NODq8%2B2GClHxY09Sgvr02MZQmBfNDjCjxZ3JBjqkAdh0YC%2F3QGeAszjO5sSA%2FYynxlMVQW7PmZNYODs%2B2strsW52l%2FC1b3I5lGL7SEeoCDfRlSlQoHpEOvw3KzEdx2M1fK9aaRqAtBnWDUEr2gecsp9cBUoRuGUwqKKKGv4aD9fEgOed0k2JkCGVlFMm%2B2AHu4StgadOuD%2BmoJXV70bSk6X97SfSmjUMk8MUhVnDsW9pfvCW929C5A49PBAhY7qrojfU&X-Amz-Signature=232a4c043c193b3f7f19a19eaa29e768683897e8d48e8950700249470e21bdf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

