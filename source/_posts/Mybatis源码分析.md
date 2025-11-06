---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVQB5FCV%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWi4M7Z4YZk8R2fuidzS%2FCYTrEfiyvRs%2B9M8W%2ByVqO%2FQIhAPAFK%2FgcxtELVnN56EFaN4tKf0WA8F4VIIsf0oQHycVCKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwjAPm5Vun2EsyG7Pwq3AMRT%2F4kjUZS4cBNunTJgfXilRqQa%2FLZH%2BveLTE4IVhnNdQACd7hmArfzH9M8l%2BuLz5SM%2FeJsOiHeMlULZZclLs2Hsz0Qld%2BLCQvOXUpjjGpJspdE9sbbgL44tGWsMdv%2FG%2BpHTAL2VPoduV%2F5hIlUAX6F1dlhmoTOJKd3iyVX7kfgAAJLyUWPtd3erU%2B0MjrQ5f4OBURvMgQWi2kNhC%2F4LFIbtBAKhizYhYPWAd7E0Fo5Jh%2BBrFCEwU8eLvTXeVFSP4z%2BRBq83gGcRgpTcXZehowZEPj%2FNbJuj8gmwMIZn2b8gmQ1BB%2FVCotLpxgS2EdFJWtADkGvRmirSAHkrnv3gMcK4ETptUW8YCRom6NhkiWbXkwwa2Sc%2F5abkLqRKndTWfuPElVYvTSWW2KP2bdp0eawRMe%2FLPfDg0NFBa2Za%2BW9MYYMjxtIjPIptvnymRadPik2aPQhlf%2BCgIHNEWx4xUEGd5RuI1%2Bcpl%2BWT%2FRqCcI9CGPP%2Fgii%2Bm35WMAqUdcGbtd2q0ez%2F3z9kvI22Xy9xhDdAC416oZheXKSgE5JvdV7nAmym7cxMykFtkqoFCJgFSBLNc8jqJgas%2Bd1cQKuRJtXZVbrKkmdLORdLEZozY2Sap9AeEqz2TeJx43NTD6%2F6%2FIBjqkAXgmf6E9B9DirtiG7VcHdNRR5RwebzMUzUU4U0CU6dMiOyNq%2BR7AJOsBSr9LjxyRNRSBs8U70cYCoWiZhSB2uj1YmQiiGwS3wWq9fmkK01ZeqiQUzEKwFa4Yh%2FQo4nvIbo9BVTMwIUiPqrEUZs9j0ePweWXtw8oQ7Bw9MF7%2BQbwWf%2FNnkhmo17agMYjUEjN083gEMBHyqSMouEtirUDTHQaNsvxB&X-Amz-Signature=35662cceec64f5233bc7bda0b58df34ac24273127fd3e95f23731562de385a74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

