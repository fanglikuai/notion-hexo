---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3RNPSHC%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQDPG%2F5Sna1A1%2BVte2LhNBVrlRWX4Olx9TqPVs5E%2FMP0IwIgSVURIy0FOBSjfDnPuLOE7QcykHkbFWI6kB7nA7U9k6sq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDI2xrquAzNcMoUrw3ircAxOx09oqUGU5GnuKoc%2BmMkUnZ0J%2FZTsJXujL2uZr3tlhQzMRsGXiFsfX2xXPDwrveC2uPHwaM6qRo4zhBj7H4%2BjGiyDdtxHoMFXAYcorjjt%2FIeVtbHP8XgMF0fpZ%2BxuMN%2B2cVAffnOD7QIcpdK1yGr1PljU8F5J0K2QSdlO73U26FhYnwwImlJx4FC9jgOs7FNz6yKL8HCtekJ1WhEzCuEykoLVONYmfz4%2F%2F62a5GyIVXFzsBweT7NGP%2BMTAeqPBT64CY0N2ily4blHRgfSA6z0zJcUbdOrMiZ8EAgMkZ8YbB6JDvxU1EKDvvPxCiIbjP9D9C4d2m2qThsU4kz3zeyXhexoutUWU5ecB3RWOmZd3PsdL%2FjrUw1HK9YcuOPoMCcl0l9dqGXriYLVibwqI%2FYcu0I2z3OYX9%2BLj2J3Pva2kveY6yAb4RHBo%2BnaVjCVtlQKFRoNfXF3WWcW5LRQd%2Bcqpm%2FiODH3Ff%2F7TeIRahQHw0f7ZfKkJqY6XCnNekFCMsY5hZTbRh%2BwC4KduBX537S6pUeAqiBj0Yn%2B5rrbbEQFyL6U5TDYxyOgrwI51ovdNIYF%2B0Hzavssm10Wb%2BQMI24obOnRZZWGHOBXnC5mxz%2BqUBXmB%2BDUJI3PID22ZMIDhlMgGOqUBmiTVuVApx3vY%2BQifWuZy3yVDoIaOeWHYAcoUo8rsTVSJS2qhRibbH0pSo687jG%2Bo8KSuQgRwzjflLpz7aA8vP26haeHCzcuOIgzlY0%2FUUK%2FkGdNGINRAJ%2FA4LHzuRNlCzrfVj5D8KQ4MJb0Qw0oPGAUv2c8M4IXmhBdwkfzpxoOpXdPe7nDmlcRkY2rpHrgwMSqr07yXmCuHFVpAcJ4ieu%2B84ZrP&X-Amz-Signature=169cd163f49e08592be870548920c9ab78be97005e0b532e380c8fa6fb80181b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

