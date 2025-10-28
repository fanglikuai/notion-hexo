---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z42A7IYG%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDkceeB0jiBNTov12bqwQAFHrp5yzTk92VxoiMgX%2FhlgwIhAKywiVaGPFrYjP7mAZ95g2N%2FGo1ReIPcuUww5%2BooC%2B8TKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxHcnRe14rhx9kPoqYq3AOz0k3KIn%2Bav22auNeT6Z6eN4N0tW4C3IbGzWGbB17BmGbQDQfvNeZIeJcpsdpbnlH60nFozML30Ftgh6UsJyQt%2FbA5JHIUP1ZInwMWBty6GuBhFV4HlrMnWehm%2BZdSUab6x4lvltdzYtohR1znm6ndemgwPTV1W1%2BdSuT7ZRKSO0TucGMn7ZB8G9Li67pTKDhgZHy8l%2BBFlisqs2sh2qFWxotKKHYjkdVLoIBr2fzC0OfRUC5%2B751ziyvTaxTdNDExxsYwMGim1rbnMQ%2Fep%2FWlbd6XY0yOadFi4DWjrDcUsnwM5ZNCyMtHboRYREpf3%2BPbfIhj9S0RPLtBDk%2Bwmb88wsDh0Ju8MLwacjix87A7RQJNLPC6rA289lOIuA1uZ7Q%2FXeOMPyvsRikEo3mmP9VYJW0yMXe2SVy33dxOTVDmfBgUIHil%2FrPF4mCUVGy%2F%2BXfBuuVq9kI8A%2B7Q01PJcufyIwZvncZn%2BnjHcBeuo0LXovWPeneN0RwbIoIGEuZ21BMXVY%2B0o0HSAB8qDGfH3wj4HU%2B1%2BFnMbEYi28SG4TtQisg%2Fwuj%2F7HrQ0guWH5P%2BnBiLoCtMKDnQPG3r7whEsescpshWazat9fghRi8PQBj8fy8RoCnISeqlhpXxNjDmg4LIBjqkAWMgPqoBxkK47c3OV2i2dtRxGZA021Rnkgfupa7u4OiEPhFnrc1%2FAZS7amB9MKSkO578rPAAmQdE6qeiWYAiCiCKYT5U7mywQeKGw9lwu60NGCBc0Qn%2Bt%2F7G57k9GE3a%2FL04wBWeMWe83U4iAVDzJ%2B5Vw%2FBW69YD7SavpNZpH4TqctC9ySQZ2ixE4g2mXV0CymbNS2qYSuigtituKyKMkJQE4iW9&X-Amz-Signature=b07a9e5d1fcbca65362ec433e599a3090bc4799f6b2e6333343be2a6d2f0ad1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

