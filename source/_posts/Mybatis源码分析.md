---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XK7BD64H%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5gPTORWipV7vOqa4k9%2BofArT7jMM3IEkR6woyxKcXhwIhAO9kAIuL2eWAawFjnnaCJTFZoq%2Fjjih9gOyjovbesnBnKv8DCFAQABoMNjM3NDIzMTgzODA1Igw93r7MGCpqm9yR5lcq3AOxpIv37UUGhrirkvSbG9zmLstyifO2Th7AsoRDKzO86OsX3PIZvU8AM4x9F1MHaNYEZY3spb26x6Lk7UyX%2BXituCpTz2LRtvKG2AIl1Hohf07v1evg1tfW2DLlIyz4DeSLNP24wZeG6ovJdIqjqd66gmV06CIW1r1nMzZQUsEYMJNzcGc%2B898MgRHnghyeA3B09Vhiq9WUpVewLGnCKe6YJ0JLy0suY%2B466jNWEEk5hS3NvtpElz4SzOx19K5U9lfO9xkd6NxutsjLeVSBXxJcVRgfF7AHisq0N%2FUL1wxpPMimRp0tgysVR395VOSdK12hNDE4r3s0TDNgaIxa4nJkNP4T%2ByEpJ9PI3FMRTPWhKMqPsYuAyHnkSH%2FKDD5y%2FXi01RnEuYtezaMDGHwepuPrGKWVmCGUJRnkelX2N6CnQqOKOS7UCeHj194uVTqDEv6ztpc%2BP4lNqNBQXAWCEnxCpoXWIi3G%2Ft%2BYSJstuw25orzu0UEpFbfQea%2FE3eysBHKtn4zqp1OaGrKtvVP577hes7CnhGLSLwyUBaZY%2FHoAkgjkJc%2FEsjxtGGFTNdDUmUM%2BB7UDgmWPoDsS0fu1qHkIJoHQ8NKLebNaid9DZMlilvc7SFQq%2B0TePEOYPDDxhJDJBjqkAYBM2AJRweHUnnjPP3dBGvsvEGAw6o0z758lxNGUmoSUSrh0jYSlpJXz00uasId5hqdq1GYDHjy3wVhjUqqtsmsujFTIbD5auMBi6kuF49RzfPgdm4D3FACvKscfk3LrW48%2B07Dao4AscnnXSztXRtBiQdkIq3tBwjbluDH3w3UgeQch%2BQuiXuSz3wq3mQl8SVWh8d702urJHF55YBlpSamiDWvp&X-Amz-Signature=06b150bcf9601c0ec972bbab29afb83bfee67badbbbc9687448063494167a55d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

