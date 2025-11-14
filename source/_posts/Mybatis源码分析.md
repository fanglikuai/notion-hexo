---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYOLKFHL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T200131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD864pbWMPPHWYsJng%2BdXl4lLysMYfciPnlJzPG2KEBmQIhAJvPj%2FlCszgF8aXU%2FXM%2BmBUC8aGUxkRzt9%2Bao1sIXhagKv8DCG0QABoMNjM3NDIzMTgzODA1IgzY7ziP1ojNZQI%2FGaEq3AP5G2KCrzFs%2BP3HJvJY4sAgbQo7r0j6j8auqQ5FnzZOwQ0JBTT2%2Ft88BcdDBCb%2BjS2rhP6VVzDeiMNGoPHFs5q1gTZvyWJz5ZYnk9GiJ1HEEqH%2BYXrx45u%2BKiKGKL6ksgntcdx%2FjHP0usohM8QTa9S9ApvCX860IoYQA8i0IVLacZo7%2F53mJCM7eynsbvo86YqxW3SxebvCvlYyBIJJ1ye0PZtdpuK%2Fn%2BUPmGXSSg%2BG8Vpx%2Bqw%2Bq5t0EDXaMKAMYql29Lb9RuRptYQE%2F%2BLagr8QkcbrmZtFzEMjLezopSfQXxuWUU4pIW%2FWV86QzhWMucSRtR1%2FEf0TN7ZY54JmvHrcTO4ftU%2Ft3sZZt9znAz1yXptJrEsm5olSBtETNbBu20dD3T9l%2FW0OiEjXiLw%2FjF4XiAS99wLFmNXipHIpjb9Q8iljI0pPzhLMpVt1FE0lj4AmpllH8IEBa9CsnTPAuT9vdsHz9hwcypKXTKEOfzQtP1pR2nQzzTQujObXM2eBegKUTCNPwne2SbU0Y0zdtY%2FlGR%2FbKx6Zb1Pt9kUkH5qWQM8KQ9fQO40Zw6MzN0AOWMN5nxa6tS5c4hrPyVhj15tA2KfRy6a4YQm5INFBwYfjN5DIWK6Iql23vSXJejDOkd7IBjqkAevzTED%2B%2BRKDgv7Bkrl%2FZgfCOSqoEufUGqPDhy198U23OkQbwmyxJOysKFR6I36bw9pqyGFUX%2B9TM35Ml%2F04oueT6hsYyxg7i8pM0WYoNWYsIjqAHTGKx1WtY1sdHQMMdPH1Iiq9sh1xGXYVBeynMa9OW%2BG6rpm2FjJq8vjKMHkiTRdZquwW4xrjLwHJULwQmqHD3RaDDmtnYqKdlr%2BJ9Ob0IjxK&X-Amz-Signature=c97159c940fdee323bb818a392acaf9f641a4b51915a601e5ff31d634de1d3f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

