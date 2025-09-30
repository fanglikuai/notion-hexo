---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KKU2XKV%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJIMEYCIQC3FSfc9FqPi7eDUNh7hZWsSP6TcOPsAIboaaiKc%2BLyogIhANat2kpTdq%2F22ePWZ08nWfcRwZCoTySX%2FK51Q%2FE0bq3fKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRYxcfRjWzn2Aw8zkq3AMgEno%2BtGZPPLNNxiD%2Bi5tfbMHrVkHvW8qqWWOUTehkUnsW%2Bhu2Mj%2FJXoPARRfzp6rjIVcJvJAXJTzalcHP52%2Bz%2F5w5DcFtdMwBQSGEMsbrz6Opksp55ehYZU2Yw8ei4seE3TRoSjPlVphLAP0iIhZ69V8EaOanR21DSRbK61hfWU%2F30vCgxYVuXBdtNzhI9DXDOEY%2FCqcNukoRSuoNfPNuI5wUAr4re7r9M6vbiG43J2wLT7ytXtJ%2FuU2zLzQBzAhZqWp962J6Po6%2FNoxie%2F2msKlbsaIGBKqyvl2cDHkloJ3hYV6BFGipXDv4zT%2FLVPEi9VGAKfMynRUQsxZmHG2XOic2Eu9WLeob6Z%2B7%2BswTRLrnH6WVeEuMzmLb7fHKtoJrsLemCeeqh8JtcDpUzoz9yEYf%2FM%2FYTY86hLVmVaBL8R%2BEXueQK0fDgsBbEcJYm6bb%2B9vX%2BZ8LiEY7VZslKDe3pdxKJ058csPfmXESmNPKw4ctY1BHxtjiroxE8rHSWi%2Fz3IOPYX6UxrosQZrpvH7K5sjFxIl%2BK%2FaJMGb7IBeR%2FgArUrq7tIf49UstnpfDIktR3b2%2ByNV4ARU46xyBdcA0Czx4Y0pU5Kkgkmmn54b9osqF3rTtixg3Xc%2BWwTDVnvHGBjqkAcvh4tXIjikCeoE53jgX7nKPr%2BRHFHEKOWx04t%2BjFIXxiw92teR%2FDF1qrlodhOvJkRfpNRG5Yk8Vhbzf42XdpAqFp7OUljxuvJXEbtsrg%2B6vWXXwFhDKX9NR0SKwUUfKXpYrRPL2djbN2Kvk2geDJEZGfm8Kr65JGHHUiRi%2BzreYfPQcjPqmT8JGaQ6e09YG0C%2F7ulkm4FdGzVLS0RDYHTZ2CRCc&X-Amz-Signature=ac8e26e6ed1171574c1d851d4420bf9495adc71582ac35c8a2352c622c23e4e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

