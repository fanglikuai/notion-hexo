---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROFCBIJI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDSTQ9aM3f4M0JXEO4Y%2B95uTlxCATrdrgPZlQgM1xtcMAIgSQI5kZJvn8SFeHr1S4WM3nb5Yi71AtOI4MrxQkQ0Yuwq%2FwMIMBAAGgw2Mzc0MjMxODM4MDUiDHMulOPTQhLi7x%2BegSrcAyQspAeD601F8yEhnC4mmFcVC4kT4sIV9DtW4nCchbCGRqlRtxpvvqMrdKNqHQ9AuoOfUDMos4o5BZvlW9BfNU1V%2BXrDaQesFPVE9NubKlKo8sfW43iRfoqsqm%2FrUNdmci1PCmnOjXlKy8xeCYforagt5XTFottJSMCiAer8k%2BxK40Y3JEN5CiEeklEbEm%2BZ1NH3oyJ5ZQRpEtweiNj65juIgu1TISSXPgMkDT93ynJSkMrnoCNvkYWvCB8q4yxnYwsyXWnDisQX98HCenxI27EbMNSh6z%2FoOPDfSu2ddtMNMGsIQqGFyeDo%2FaCxcmXH1ZIvzJk5RYSCFK7ZSTaSvjqxj6H0h0Xbf4M%2F0GTf0kouD7rchgDN81hh7foNbmieHP%2BoPCuUlCUVnpNgKpygTdY7Ngiww0QarGGDxBw1ujOzsTpvB9pWceFxtJRw%2BRhZ67u8Hf%2Bdmw3QsRca%2B5g%2B39ZpYTKWdtEKyGUff7vdD2wVqU8wE8FkmWVBHz3xutbNpAKifpHeb4%2FQP8it1S6VhciZZGXecIbsUxNwoYx3yfEJqOoFgzBElKSxPS8zvQciikZNuw4tt4xyptkxtXFMqX4QqXBld%2FV5X8F3VPKOL6hnvORlx1dwUfG72qnFMPms%2BsYGOqUBryHE2wPsAeZ07ghnv4JEVApN2Cbvqcm%2FsZ1Bba7Bb1pm8N3E6lElXkIhay%2FmqPEZwUTGtF7huXhPAm%2FEuZxYoR2WsbSs9bym%2FF587exEX6gE006F9lbiiUhTFHhQIq8rvAPfD0j1e3mO5zujCJ5sS%2FQ%2BQeIA7YsC%2F9WQDDqILXqA73hpQ5o1uJDNaJrFhgJvQr%2FpL4umpd05d5xS7A1MbnodNQ0l&X-Amz-Signature=51de6fe2ee31d528051115aa15c062f9df1c8f7f5c52fa705d3c218af7f20fc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

