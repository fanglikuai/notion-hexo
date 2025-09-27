---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XWGGI4J6%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQDeFwztbmdDD5wr7yb2OtJEL31D7nBU5tWQ73cfqwH%2FxAIgdneeVfO909tVIp3z602gbpVH5Y8zXLfQb9PbfQkNdhUqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEDHektdT%2F59sIXb%2FyrcA5y2tqSqEataz3TZG4D0x8B4aGLI%2F94WAA3Ybz8Jz8UkWgPbtvicYsIJw3nEYqY9RZcOb2OOYx3YlasrrR0oi0LBlVNec0LF%2BXlK5ZJsbx%2BUDLJ9QW483oVgyEPv%2BQEBOjRuRiBO2S0TBSt3NEXvGX4qiBREgTlaNWp2Lr4VplIQWmGJoU%2BfAjFVx7co%2BNPDFbTH%2BJVe8VQP%2BtPxUJJroNqeKK3r4bwia%2BDuobBsYhY3Yz6hSd3pNNKMMPSsLQzVCNz7clGHZ9nl5HF8YTEWlt7hu4r%2BmWQGXiB1TuzseiXxmmpQfCsdZ4xDkATiPBIwHXOLZvl1aJFk8k%2Bu7m24fzJHL700hf%2F%2FtN0q2buSQzQS1YI%2FL8Bs9Mz%2BsB%2FMwSerQWgr5l%2BLNKfS%2FvhDIyGmil%2FEcouv%2BXn5ExxblawgtisqXafpvuQfVNngufurjqw6CZ1mRPw8hZYBppBoFqe8vpHA%2FWr2ilgBQqYcrlEaufrYQHT8UOKc0RZv3jJgxSmvh6zx9bgCTrhgB%2BeutRrEbrVXPTAPFHrWWi6BlNjOqImXgUhRuYXtLxRFMYbmBLrFCZ3JMTiZQVtzkUroFHedL%2F8ElaWMomzKCmIq8vPwbrdRCVPb9lpW5FMr%2Fd0uMNTn4MYGOqUBrAK4iCt6W5jWI2oRDrZVm6cYvkzEZtQ40WFwK%2FE6yZrYxUMU4dugD%2F8D8oXKMtP93RG6sOBVAOhkKzitEThzgkuaUgPPUXdrCb04I%2BhseLdRPCrbE8XYZCaBlJkeL7X3o%2F2XWixLrTu85oDU2lZcFbpOWf%2FjOxMO8Y6yU0AYOcGAv318MnA%2FqzD3KtxcQfPh3FIJGYYfrKjQWnN%2BlXP%2Bl3HzCYOT&X-Amz-Signature=192d5d6e07f0d0a14043bef6cd43cd1b831e893ed8633b38c4081eff956c27bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

