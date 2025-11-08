---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVH57MM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQChWiDYVX2Fus9lcbLQFO1eMAvMQhfhM6bjWfYfZ4JRGAIhAIOWCVOQY4snrhnutUO4%2FVx0NaW66MnlQdGoM08%2FzqCaKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyEDtGVp4nti21zpOEq3AMZ09Cp4%2FWn6rno7HTS7%2Bx5gici5oxB8KlEy%2BpsTguOD12qHKpyoQ7qBl%2BT1ldy7NNgFziZQDTtIIo5pFxGdx%2BScrS0SvgQQpusjbbGD1QPJYQF4JS6nkwHapDwnu4QfY2wq%2BkXkcG4fkZ1lqBprHdd6iyHiJvuQw5HnlDOKZZSm3zWToenj97SCI3ndaCfEMNyn5zTqvLckgho%2BXVsXKIqWKpMQYrjIvGz224j03MSpH9B%2F4aLWuBEhHtbsPFIvw7piO1Nz1pvfbFxzlCvZqvA%2ByfogCcArZsHNyPXi%2BBIUjMx3StYPwJAVIl1a5H6FueVMQpXfyGz7oMnKGdaiadmhXK0bI6E7h100geD%2F2OF0Kp0JMdS4ftdV%2Fqu3rUFJ17OtSw1btD2vppysJLS8zuu4OGHbX3rOTbYkj5hRC99ObyLFvw8n0xM%2B%2FSxn0vkZFTSNzt%2BYAdtwnl%2FjIpu9tMSAx%2FV9LaT%2Fb5%2FFZkixpWZ58R9QAWr%2FR2ALpIve4Ffzct4LL6T3UBjt%2FLKkjz6Z1xboj%2BGVPhTue21cphBEKyNEs9BaNpNZHr%2BmdGurmjKtuTwK%2BylmmhhIqg%2BXODx7cHWUnRepWGSOr%2BH%2B4LS6fRlFeJdFRczgiaWPWFUxDD2%2FL3IBjqkAVRHE%2FdtcoWpOLSHTTFtj%2FVo4gD53BSUkf2qSSmtExXvkWQyHWEOCd8463QZHcTvHNO4hemF%2FO47fvpO2jsshF6XOH%2F5U1voZ3SqLjljL4pmqIMSbFopeAb2CeXmwbQcRlUlPS8z5Bluk57ALy9f4x4pcyUECqd8DvMVADjkHqXSy%2B6BH8EpWVspHttyV7UpWcbGjEo9HydLVOEXyoXDDJ4XyjQw&X-Amz-Signature=6a4d13839bf6df251c1aaee7dc7307fa63f462c436602d57962527e9bd599df5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

