---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTUBM77G%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJIMEYCIQCM%2FI9ARE%2FZJ5GH5IeNcSJpj1OD1ILnmfnc%2FOgVyhS1EwIhALqgP%2BOPlys1%2Fa9GIG01CiWFUMI%2Feh1iBHGCUsgSn1C6KogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyh4byXz1JT%2FsErF4Yq3AMbMucf7DsR%2BmzFJ%2BPI%2FymjGH%2FpXsadKuXBEiR7N4%2BgB1HkRfg4lhlC5VuYH9D4AmTh3XWLX%2F3JPj9N7erRHKh5IqDkCvVJt80wM0jjwJmIDlh0cRnZLg4hIZJs%2BAysT81j%2FAm6pxYCgauOUe3%2BKtlQlGzw2oh3CLm4o40bF7ZxzSjN1ZEwHMkEkDT%2B0bvvO%2FbLaUJXyBeLOqB%2Bga6UoSl6EucrXxHEG87TG2x%2BpDYtkyukeMF35rgdxJHCdNBT6lZvbk0Vh6%2B4uBiHqw94bu7pMW34DtreeR7bLSwU51bcitq2R0HI%2FsxLF7q7SiGQDQrKvJj%2FkaH3Z0OVWIASltIT0gP2z9ec2%2FpXikGGT8CKZoA3%2B5j%2BpWaRt6EWQWk%2F0gtAK8VF5JQgResfcoe8gxLgt2p6W1Qik5yxrxa3cDqLWib6G6OY5uRxOHUGTc7G3r6rU%2BY5%2FMSXaALI1ZZcVTu2SaQyLfCwVfJ4bMY599jTohp0d7O5VuGZZ7N9Nfj3vS0PYLzb2aCctA64XVYLXrKpyBOiD59BaSG1ZgYxpqjNjG8EuuRWuyofLeDSaVTuTmBzAQ5m92IdnCywJmlGl0kjTi3JbzRz27tGJ1lFVmWXPZTaF0VEIb%2FKobD7mzDC4ZTHBjqkAYLhxssHO0SAHzFkUwu3DaohLR0A%2BDyTBmTXEnGtLBsIDjnMb47Weg998CDXurYeTRJe3JalazTltCZxGF22xpqBqdfoIcOskNpOPBA9CEE6LGowGShZu2oiGBGSPE9U8dBRi2r4PPb21u3YAEvYjbgSEMaNvRfob02JFdMqI7hbpfAeLTLhKm0URm3LRU2HJ7fJkyJxeSqjCwYe8ZcJb%2FORSZt0&X-Amz-Signature=5695a53619780f2222302efa3e3239ccbdc1ea0bf20d355a27e1af69dbd901d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

