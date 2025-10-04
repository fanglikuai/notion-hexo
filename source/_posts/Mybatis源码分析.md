---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627HB6MT6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRCXQYcQGWBQoQr%2FiMvkgolBNozJocGhf1vHFCls01%2FQIhAOqJmhVSxjMPUWsmGRKBsg%2FJzN4GZ481gkwLWnvM03mRKv8DCGEQABoMNjM3NDIzMTgzODA1IgxQMCe3X%2F%2Bi2Fuit5gq3AN5ZXz0DovJY9O8gmQEhb%2Fk3RCcvBLyxS%2BkNtNNojmwVvk0kbSeG0sx%2FN3FKkh0fAQufFzD9csiC%2Fzm%2FpfR6wVVfpUkeBESqSXaD7c3Yhwhw5iFop6wPuZeSXy%2FMejJEoNQCmp%2F4EjMsuAaZ9xfLi755kV8%2FIoypGYFfn6Qn0oS3%2FPEvqFngXipco%2FrDICS5erd5%2F0mPw%2FHD787xhHEGcAydQ6mb5I9kj8QunREsMnQuBE2CVjvVYr8JylIWB9kbQ8Rd63xwYoUKnG9iQmAN4ExRFgwVbj3RVJ4vUXrGaF0IVSXoX%2Fyn4WTuEI5fQyi0Ml%2FwbVubb%2BRukbTzFG0DkYAPaq1ITN2M8TnCm%2F8qKyYRjqVDWNvIG3s9zXTzt3bn8JvqrYys8yyASg4vdAqQJNIouUE3ZmFOAbXquupOU5gRIJb2AgoYoOhHFKe%2F5RibTgQASF1RQFIz%2BVducDAkmA0Or6kgUNnWxhpu9xX3JvdDxbRUwj1YC%2FCIECUWtcHrOZTfTq2dJOidQCmSCeRx7L%2BHmeC03C5OI3c6QRrLJJaP9gRgu0K7rMUJ4den4jvKl69FPNed%2F9KBHoG7n%2BkTfUB1ZkM1bPtMVyPfDEImNz9let3aRvQ%2FVsv7d04GDCUj4XHBjqkAdCoxUreWMg5fNCF%2B5rsDERfNLnl1m04SklLwTHMhIttlFDC44FsqjAzxsYGAi1b43uIZOU8p5vLjLrbrO%2BkTZz%2Bb%2FXlmO5bX4n1MNd6vh0jgvwk2kTFdasDhtlfMLsJiRYg6jVkQBQ8%2F3%2FC%2B%2Fae48q27GnW7zm4cVzc4RlYwgbWzQhmS3mwxrEWGBm5ngeJWwcgS5%2F30ouIKt8gcLEwQsPY6g1B&X-Amz-Signature=91751fef9a14428cfbf02e9953cd0c9519f5f82aa9c6e66286ea733d20f6e36b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

