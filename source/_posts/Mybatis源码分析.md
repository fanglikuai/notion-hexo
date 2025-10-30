---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J7NBSGB%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQD73DIAzUVLwLcO66wsx%2BAHVqurBYGX%2FQX2EdqcjZWBSgIhAKoWEwf6khhqDLV9PfAWCwUnJejUm0jLy5yjhbkQNpMMKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzH4f3c9%2FoGf2e8LDcq3AMgtBw3ZUg9ZPrg6BZP6oXRpB8qesX0o%2B0oeVjqjsTt3bSFZBgFmOBME%2BCkO480ffhCXH3GJYXvrxpe3Qz20SAOdzl8X%2BcJuhpbPR%2FmOTPk2RxlGqiaZ%2BWxNGjemKIlj3ZIqOflVcd1zWNgJWxxH%2BEXD97gv%2FS2E6K%2FXj7vtIs02rjq6OS%2BByBbmQrvdMK5dRIteQ%2FXOoPj87ZMKWAJQ57Czw5DYJYoL51bBUHR1Mj3UAmHzH98vNvnCmCuX8LP9VFmuxipHNbcT6lg1FGvl6RoKYJ1vWgzJPHrjc06Q38XWTQhvnnIBdahARRQ9bD1HzN469AYRMDW0%2Bo5urxmAAu1YBgoQ0TTgYFIj0QXWH6eNo%2FW5jZHleXYjYMEKojiYA97WDASHjpR%2FnZ%2BFoptT2djX68pDUJLQMbUdp34bNJRsqFBTEJGL24CNhu1H5MYU5TE5oog8d14lRY2nyqxj5sJqkAVBZYbyxXNb5i%2FTql9BdB%2FnO9XBHmTXr0mImdPU6I%2FNO%2FBp9B8Jwh1KcbR1uePl1bZx7Vd4QmqedsSOVAUVcmo4P7ZNdEDpdW1MPvI4H%2FmeiGkQnKUCjiJz%2FMo562ajLe7la5y2WjEuMrOBvCspP1JddAO6chPnES9lTCxlovIBjqkAZ0sBlmjswc2cWfN0%2F3TXyaTdYxEVnfhaqU8PxrwTzdh89phkq0g90sFa6IugC0jNoHrH8plEmKVIyYNKCeqxZ2CHSPBbmHn28b7Wl8nLUabz2cmnpTP%2BGqjqdGmZdPE%2F0Jy4Wa3mnkR5nnXEVcYnGK4VH01ukh%2FqXFtzTxc3Ih1SxwwL1prHRKF93cEZQCAaXLwMMKJREje%2FwXFcavcfTxRL3ln&X-Amz-Signature=c81db478317043366650dda677e77d0ceb3ccfeaab8e374442e90b6ef950fc3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

