---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIIC5NXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCDDcOGDLUiViNC1%2BLr1GMCUTxWbN2sTFxx9fI5Dat3WwIhAI8SHORk83VSXnK0WKpMTU%2FfFbiPpEgL0V%2FRytok0%2BGaKv8DCFQQABoMNjM3NDIzMTgzODA1IgwVddjomAn91FO8eXEq3AONk3S1Hf3rPyoVFKQddcvPdegm66UySniu7JwByPR5XNhUc2m6CsaAC%2FH7upz0yZspMjF1cQjvfma%2BM1I5NptHdL7fE5LPUzgj84Fo6BEqt1wPn%2FeR4B451463Sis43ESQhIKxYMIgSAhwYkwPa3CS0fZ7SWpdv%2Fm9dtDE%2BQEq82DqfqXw7vGd%2BiZJYMCZyB5G2mwe0%2FUgTn9qBaE3DwMGyEgeY0kYVHpAeNf4HOS5TNbzaY1LA67wYDJ7fpgRVmUXD5DRObJDdTSgrba4ksPNO3XfEgsHSte9mLPpznvvAivP%2BvZuCKQ7leiVhSZWa6XChly5MEimq%2BTmOTo7XV8pyrMy9gIUrYfORSyf12QcJJpgP64CBB5%2BWEeJFQKmcGi%2B5B0evThjeRQDV5kuE9BZ7l0KHoqj%2B7VryhqBObGUzg2qK1vm5HW%2FI8ALxZxEc1bMMYUxaH%2FiCd4ZXG14AjDwTZXE5xiA4euDIGXXZ83TQLYpSDeFSLFXQiORXazDDHZ8%2FVOFx2mVs0J1QCCb9Mx%2FM7pYr8qCp4OX9JrmGydHAGAxWZ01WnssMu9Ul27o%2BFUb1Yeb2qwlJS2ZRWGmn%2BUCuSKcCS%2BL2qzTSK1TM4azNsYe8WVr%2BbT%2FYRf0oTCT0NjIBjqkAVll3gtRuz676hxqNMZo1s1osL4MpCKEw0DXzKeH%2ByDnxMXVE4%2B2RXvgmjemdBWKfsYj2JdwC9uJ1do30F3kFAkphxu1D2PW7Rg9pabmFag2c8ceAd7wkDS1VVIXsk5eJibc%2FIvOUFOESnLFVuzQ3vFJMDamwKZDr%2FQzMwTzR97NmEZBnStPzTptHmMACsbVcrdC82kQ2C2%2BaR%2Fv9Lm9vshcnzvg&X-Amz-Signature=fe68212896cfe0a213e05bcc9b5c30a036fa63181a4c48247da68912240b8127&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

