---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BFEQ2PQ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIB37WkeYaHPBPUhWQnBxTDMdwA%2BSoxO1w6dxaU4LQ3vAAiEA9%2BrPWNFNbVTLNjZtbQUv6xtxsmVQwSVMrcVgYIe%2FLskq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDEUrnEFEcWozRHEqgSrcAwdzKYAKgbR5wl2smgzf5FguRrj%2BlATXO3f8fak4aztmk0eVONgZiJgC2Ux7MzfFd%2FpMX%2BYEFp%2BtH9OJyU8IiTX%2BTmJIkkhh4hARwU%2F36tEs2drPNaqyAPqwGvAf3f2W%2B9yg0LKLuQYlDTJEPigW6hagRV%2B0JzMFLPk24T5h5cNx%2FAwg2yqKTcCNRw6XY7KjpwCIv7xAxLgpSEDhaYqUPDeVT0%2F8Uotn%2BeOupgT%2BMj%2FbPlDtB%2BUqS6iKigHg07vUD%2BHlAYWrCAvjMJpdCPl%2BY9UFE8UBQ4HQOSCUsfg3bHdO6P4St3dhwMdCMGvh1Fnz3i0jcAZtmTlpOJyAbAVccNP5bOJGAIMJsqzn74zTktPWMUwA2gdc3E0mI7nK5QCM%2BNm2YfT1etYTawYiUa52SsQfXvr5J%2BDIEVcTeDWu5SHoB78xSbm4ZI922s0oiFOoYLkq%2B5Omh%2FlWtQLPXO5%2B4eJbmkz0595XHAu5BPAkwrx%2BDcZ%2BihrFwsw%2BHpiiGAZuv6xnyJmDdQ49RK4tTlMllMYKU7S8mbuu8BPTWLMqzwGCiOMHYHCiYFbwWgQXafmWXA9OQI%2Frlz53TVq7DCjA%2B7Wy%2FQ57xLvDtEoVjWwo1PRCtiDCJGjws6JS6DurMJ3UlsgGOqUBTBszKC64kBL%2BgLWyR%2BoUtu%2Fm%2BxEZkUAeuXPNEfLsYZ8JysfyNOgXGQyPvwm0AcOMGSXydFl0MxuCNsCUCjQRgJ2FEx5REFq9oe%2B%2FPBWMGhLFuBJ3nwpDPxGrY65tXeqpH6fsI3jBBjK%2BfjtAlNUhbgynpxtcvFLi7uTXd1Bv5MvmJ5T6VqKd%2F2uhBZVAc0FX%2FK5DYGN0Bef6nxxLDGCLMQDyT8%2F%2B&X-Amz-Signature=d2796c7880a5649b0223de15f7a60eff3eb2cd4ecb88a33cc839b11296ac5159&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

