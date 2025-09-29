---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IWAYUKE%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIGOcwqyEN9fKtLShdf%2F3cBH0sFpJqr7tTkfQc7M6ZHLcAiEA8epYZreLvCQT3dUtZcpo88hmIWFD3lRLUcn6O9WKwRQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKJmb4IXsKTAGfjx7CrcA16ch0lRumcAjIEAIcD9pGqufWcx9KkewMXtuoj82yEDOAxCE6N5kZYZdtRZFWMlOGSAUTOlndg%2FH22S8%2FQEw%2FDaU3pwsRyTykG9YIXvq3qZixQLt%2Bt5LilBG10%2Bq4pDswOytFX%2FCcaEcbasJW3YanhgRVemxrRDUwA4IyjruMWGp%2B4JtkraFLx4fnGSJFDhPCX3q%2BUjmW9RfoOwOWv5YCgDHDfCo1jjutkyuMi2jOesxlL8fleIO3b5e9uv8gzTjtx1%2FN%2FTkBIa%2BfZH%2FXK6XAa9cTRcXsTNNqasPzIlVKaIAVIUXJQKZw12aIYT1LJ9FqOsJOvc1lPfO2AJ0%2FkzyphjzAKn8VRICYXXq2mpT2L2ymyU%2B1B03F18UgT%2FrFxKB3qq3IvmKcvMPcRLrr5jsbxELTFM0qKNFdpceUCHiTYXT%2Fc0N0NrWZZYz3F9rBpEyIdSweyi%2BtrSqSuLWQ%2B5%2BB3tt75MQfrZpsk6U6WrxvSX9112uCUqIM4dWuWuxRLMLL324SGpG6bgf3wGIXZNVNIXjSRuz6JUKRPL%2FU35fzu8cwizqhD4ARVbo6sJ%2BWhMmyG1mFYEAT1nt42ynTFnEd3%2FBX%2FqknuvMngNu8lXlG%2BBDgZfPnAVXeuiTPL2ML2r58YGOqUB0SRm4r2pvu8svgVV9YDufwagpF4xMADAGtg4AYHd8jERAWBS70DcXIIhKTjs2ZH23Hwvw%2FcN3WCxkpvnXC0HgujbueFZojGCqC4DraLgEebSvFF6YOALEXSRVeIIMD3bVl8PiLVfnvBCrOfmQ5J8bZyASoRtpmE4gcHOQm%2B1rqOrDJ%2FWOKQXa%2BimtfSkm7JhnzfMO%2F7DzGGT3NLohb9lesiP17X6&X-Amz-Signature=02e21232b2ffd2dd6678bca6c343aca869d8eb5a5f0bed7271a01a25c3f2d35b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

