---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IVW2PLT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3qX62uVohAFoK5hj3kdjDnOxiIzlfAR8FceGhLvpFFQIhAJUunVFnMgUCNTF%2BNf1Ad88TDjXvKXnhlL1LNP7qRUJ2KogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwMlFFYuytUgNWHV54q3AOvZh2ljOQssSbrjl3LWCYiaN4JkBKKIGP%2FKWgAGXLQQgJbZpMTNIfzu2LopMNXTnVhafrSKsy1%2F%2FVXJyTPCl3REudMNL1Dvw%2BHPsPMMIzXZhwK19n975FP%2F7zh5I5dGWz7t5e25%2BiC1zuGvI3ZhDdGeYioW2zsSSVBAuxxKY0ZHd58%2FzTDxNIF8dh8weG9mouwMcG24XfVczaLbULWEiOkdv%2Bmu2J%2BYgCBJ3MvUnxhib0ev0%2BtVREYKrrb0atWmGziLb29YI7mEISJJe0KxxHv84WTMgbprWqVw8MFqOkrN7ReUBDT0OyIqKP67P%2FBLj3dvrBDMSIsqoYMdTx%2BFVRegAyBEyNN6j0gzshLHEaG%2B4oB%2Bpzk1QX7VxB4yE7pe%2Fk3dfKDZ1pDQd9OrA06TDeYH0UfymjKgjL%2BlDxwZpB8hZCwCYUTKtUoWkRE%2Btcz007Zs7k5A7AKg2dLnmGENWwgfBvXfJ0BuLuwYse3x5y4HNIr7fxBl7LxJtJrTfqUndtiFzv%2FpSY%2FFLcQcX7xVjiZuAefcPje3M7HWv4tqEyacqdgjQOttudtB0ovPLC7ZWXFnf6ale9Zmfa0Mcxy61P7KgBBmVdK7QXhx3KWBjVOyhQQIFH9GnyOI8w7YTDWvo3HBjqkAXeNc2d06V1WtC%2FhO2CTf8X9%2BsM85N%2B%2Bwj%2BtiC8BICT5%2FDc6DCGIe8U%2Fx6qxmkGkUkcmhFEnpbGHlJBGtud9QfbxPmHlTuz1IgPf16EGe2OKjTTo%2F80TDFO%2B%2Fb98qlzpaL%2BJJwPNm%2Fh2Py58GjNRhcg9aS3xwvMsJgpH0qHle2zUQHWC4ye26tRGLfZLHHtRz00LC2udGJMjJIEN3qbfROQXF4mI&X-Amz-Signature=28e39d159ae2f31b3e39907427641d304e310032b2632ac268880eb12b118dc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

