---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Q35CO2U%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T050110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2FtmNh7ATe7klgqw3VBy8x6oqhmaBhuFnpu8o2cQR%2BogIgPlrNgr41CKGWXn8yKMjFL%2FgIpRH5yqhG7PPNk7N94%2BAq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDG%2FUpyCUADmzLiT4%2BCrcAw00xaX02SiqcUtsqnfwybrC5fV8n2UvCx8lzp7hnZfzta60fcX7FAQRoEELA0OU0rsG7%2BCd0TUCsLXf9Uaso6DuQdFl3mPVKnOnjvXiCuBP5wUSM5wRCbAdH1QLF89sIEYqcCVl73pVDpPXwEOddmK1zCfJKUmSn7X9Fa4IbIqz%2BpZsD2%2B18cTcTn%2FoWtrwf0pVUrgxcLRc1bB45egoRVGsh143zg5wSqLui3VGdpqA44SqobXKTsfibU6TiAwy3q07WpjNeVi8mVBvl8wTeahWXt12DLH6gU3sCk8p3k32Nosm%2FWdZ%2F5Vx%2BJwkiCenBwLXUP1488t5SMUcS8e455jBKzdHwa1YY5ymAl9F2LyCn2iuU7BPzvSw%2FXjgbDHUdBnks874ah0pdbymbts%2FoUEkS%2BcdH5f37AfUMv2H3q46YWkiWLR7CdoOic3cXu2knRzq8WJEoE8TDuFi6Ygr5s6gV%2BbfQnNiazcCRtpH7yTeFDUbFwi1vJwQsx1XA2yY3j%2FbpuoWXhWWWpOxKsjO7Kfm8KcbwB5Pl7cW%2BxN6Lgn3ErSYCoMhyE3WKJi0XiX%2FqMiDDumTdewUyg3dcM92S3ms9UFmXHuKM12VBIiv6DGgUoNkka5jH2RKRvc1MLCs8ccGOqUB8g5ceQ0RdRmvynwddbhUWBabx%2B5N%2Bfb0xjUDIFFsH1bRJl4vfQpWhwtJTIs01rmQ9PcglAbvT6OqALahZHOv7ZrTuDvdPiPRaw%2FPlhwu6s13SS6aMNk%2BU9yFb8YR3RgpxnylvGwZW3lJHMgcEZo9Z93iDMjZEVuzbzjxcule5WEyC02iaj6X%2BL%2FSRQEQF%2FUVEupvVud3O7KmvmqcbCSsHVrSZK2E&X-Amz-Signature=0b5641b366688807cc8ac3e9f11548d3387e84e0d34701fa01117ea4c941a3c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

