---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRAIOWHM%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDPzCd9FS%2FQaY9UfKCsS%2FqNHOYEEOCPYds8VWavfYJvDQIgKhIgmtHx8rvFsGauqBvBzJPPOPqRFvFQBsJ1CTxWOKEq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDGnWQTEBLwgZjgKi7SrcA34M%2FYpvQqDQHtuLNfBcboO6EhFNgZe%2F9xm2tTsoNG1521gtGVxBhwfN3Sbf%2FnrPNuuOzh%2Bs9CCZDqGSCYtomAhdor9LPMJxSZorU7GWkQMZFhkbn9FpMjdpnNycKeaD%2FPnSnHMVhRXWtUPcvP4RKVdcPbmtHlc3mfxDKJ9pofrUaIEBk8xuKJuAVJ9LspDEnhBVEipiMHWGg78E7CI7cVuaukGs3V9qmx7mptLCscy15iR4bEZ609icBEmt4JS0SKJMRNhMnEgu9RhbXsrcO9ysLi24olwueO%2FnJRKVbX8e5kuoBFpGAXLa8PyQn%2BlZobetX0aJ0CXdej9bFlyff0TLNAdJvpfcacmSjGGqRnTK7fyOdljaOFaZrLR%2FUH5%2F8aVAcbJTi8%2B3RkCdjKQoyNlzPkMLp17K3zt0JFoH9EmzPiSCYJ7OMkcfCvmMOrCSlPbDZGil3cngRaEIwtDP3gOh5jXVbMAM%2FqkDyFQQo0Rj1vyT%2B4L66YSB%2FgL6%2Be2e0V%2BItfBGH8MhY77MGIj8iCJ3HrP87BnmNJIRDSTuJbU2oY6BKZrtxWudK3BLiOvfjG%2FCtYMZ%2Fjy2EfwFiOyrqo8lGNN58POw2gJ4OI9YO0Cc1GUf2L7j16lkoFFnMN2s7McGOqUBHa8GVCJa5172mouCRp8KcDS%2BB6eeWOBrwSwbuWb%2B2BGGPUwnco%2FmGHOevWCiDiqTltIOiUnp9DqQ6rf1crrH%2FCZt6MOBonSMcLH16mv6FD1vgt83Inzc%2Fvbu%2FBrKIzubI0%2F9Xa2%2F9MgN4owxmiNaYZWhLTjbFscxasDmMB4vVYUhze2rC%2BocmTC6VNKnyntHKadWblJFNkVu7p8BQFz0ApO%2B1nem&X-Amz-Signature=70413e1f484cecb5f580f4e7362b2cae2a4d6a8d34f267f43750687d829dd8c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

