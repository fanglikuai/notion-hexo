---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7XDDIMG%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCua3DCzi1i3glBXs8jcJD4PnhZfRC8VYQw2%2BrF2oR7bgIhAKJYA7v1u37MRDdcF046n%2F1GqPc1ZWbGgS6QTmiF8iEmKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9nF45o9mKIChmhegq3APLD6qCE%2FGJab6Z4lc1MQ3Ge0COiSR%2Fdz%2Bywl5DF8hMiN4T5Y1ks%2FEJpACgTT7o3Ty0LB56RPiOA9lDfNSHj2TYU1HbEEl9WvgaBZBplzUtvCELyBVmZ47CNszLDevcUX71TNa05M%2Fu7FHyqo01MPnTG3rOdrIfg%2BdDiyK8OOgk4vcOjNmkeTSdu1jC3sEpT14J2ILmQ53aHJo%2B0cV2WZUpY997vQ9y51wqo1z6qpD%2FKpiUljnDKgwhTYNNsRxlBTNHcrWrAsUL7f7w0tt4jxIkCl28GisSzLbdvFyxgWOnL%2BOU5%2F7N1%2FzZvhO5I6baS5i1TqIWKDptBu2G5%2BUuuMeVulM1fpUrUO8Lpx5hB4dRzXq0xNimMUPuufs9c1Tgf1PWrLARcnoqEMExnz27imPFfmuvJ%2Bi%2Bai4wzzRUXCSmDPJLfr5GIPToJ%2FM9ojLABpjth2CJjbt%2F3DCQxAjNpgWVdDFcBdxQ4hHCOw1NKXaa4HtsLx5k6fRNzgQ94Q0XqpaHcoVySKFxsKpOnay4sVHL1fGtYwqMA%2BMYXlensb%2Bz7SE%2FhGj2uuVP%2BF85TdM4BrPeCcxYZ9bOAK5AuiY%2BIUT6PX2UkkmRK4SKcZ9thd0sUuSIsao0mIeNkUGVozC%2B3ufIBjqkAaFtwc8bGATjD1Sb47aIvTjv%2BTH%2Bg%2FphAOUJkpiL9UZ%2B075wGCXMteEsoe3PwcY9ZjQLxGb0hE0yCklIc0AXyVszhlZs8ofsWGjEGSwytsMhCsGV941PjVXeVRp%2BpKJyQ%2Fm%2FT28e%2BbH9hQ%2BJ2ZfquHaKfDDstwGPGgjkjebsIv2WnzV%2FRQADyVN4BIBHD%2B0rmz1ucgODjLH%2BRN8MREtvKGv2PdM2&X-Amz-Signature=4ac03a1e099fd8967b5db90a2b93ffc8300c75e589d6d643f625500da515a1e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

