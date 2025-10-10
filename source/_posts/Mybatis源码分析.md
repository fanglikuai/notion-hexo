---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBVCDDME%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T030043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJIMEYCIQDSw09zqoA8kDeeDIyvcrD%2F0EHdq%2B4d7%2BM5b2vUfOXlDwIhAP0k9qGSSs7dvirQVW32uWblBBNbm4db81nmWHi4cwj9KogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw8eZ8ngDxd7ejSbs8q3APrPfi%2FPAtLwOcGzT7VRvbMKfrXE3PEnS8d7Kl%2BkEAaZLU1Y2Q1FCHV%2BmVbncy24IXDpkPytaGt1dl1hd6tnH1s94YC%2F64D0ZZFz65Zxgji8xo%2BybMmw6CLSd58YDCTvfODcYuNi%2BihegeTYwPtk0srLeTUwBUjsxoC4XGPObf9xA9p5zO26IxvukEIrThOi4fDTCrhmPF18EXNbY3fOTjjqHK8cF3mnVA50yg7z9kwrfsqbuGhgNCB2LflXX%2BbfcaHZ%2F0gzLeIbzkxNS6vJRTbh1Ce5hw%2Bcjfyr4HdXwcj%2FCsGATn3Y8iYUtqwBJPAWJ8mEJ4Y1ei6yIQBjluy7wrKG6%2B0GRbR380f%2Fe%2BRglOSHf9lt1d2hkAzG4z%2BgSdtpIC4167QBzF3nncH8WiRCc6qiPjbHTPmXy6yzeVMX0og2%2Bi60w9Idhbd0vA%2FIWS019qQmDyZxN7WmvhYz6QOt8Aa35RLv1Svsb1%2FjzDhGd7yHBt6ZHyncO%2FDPmYQCS4KyNUvq%2FmHWb3EdFzhNRaGLNGXACCVPEJtSfCwSgoTNM2KQXehywmHVbd6MG3fNqsxLpc4vMr%2FNPnkeNRtq99zaRgfekUR7iogOsKmQSMXMAlcLXBazylVw5h6il8AFDC81qHHBjqkAfZS8n4lVPamC7SwWWWDtYFjIxI9R5q1fIWuIvcaTJ0RgI2C1cqyBigoV8tRwURmf6I5SmH0dRBmFbI0grogjwcfLmaJbCtr%2BrA1niLiudHPBCrsl3lwxtIEtJvCeCeDPy%2B%2FFX2Gn80i%2BeBYGaSRO9QWTJLMJ4%2B1C12grcGFFCTuuSlZzOCrG9LReaVRndNW2eBmcEqXd2BAENSOcvLZ7v3pfvmM&X-Amz-Signature=6704b8518726b6f1b929814cad78783ca9f082a3d3357e588f0c66f8d1058d86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

