---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNF6T3TQ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIQCuBm9aq%2Blx1Qx7vWnR7sYaHpxBH8%2Fl5Yl0gWi0yLvG5QIgKAZMbGgG4Ko1JN5xF8byTpt5xI0PB1D96zXdAzFCJ2kqiAQI4v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAaSIFh%2FnWNnbqeSryrcA23yQDsmNhqHIK%2BmZo7oNfdrIpD42pW%2B9EJ%2FDXT6AoxH3tEDR3y4%2F8IO4xSBWLumJlEDap8cAhLq%2FfR6zwvyroNOYbiBZfiMhiP4cmaV041iqSzhMOhQRKq5IjospEFY7MA2HXE7dHL84WeKItekjwCF7dvPB%2Bh59fUcdlRkvd3dQyzkwop30Fwr1df6zTUj02euhL7lJ73cceHnFdLs%2BeiwKX2GpTxe8Bpy%2BtBz70C0Wa9x72ShHu5KEo3Bh1V6UVd9c4KCwmg6jG8HdSNoXZMgS%2FfYpT0vdGTlgm462rpdfxvT9oWAv6ddPSnpnz%2Bq5jP9Dk9a%2BZe%2FIStN%2BrjN3o2bizOuZVx5tofsTIn4q2x0bf3CCyt%2BhtkcOQGeMLYGIfocUMCSU20qjbbPA1r2YOs1Y6xJOfo5w%2FXvQfAv9Rr1ihuB2JxUsbAdH%2FmKng1XNQwCdVu%2BhIHJ%2F061vSYxf0wNhMKm19jMNHSuqmgZ7u%2FNIX5bA8m74OV1dt3DA%2F2prr1ptB44n8j9VWTipwbQiLeWk0Eu5uy8%2FIBg6JyjUwX0S2Gr5ffTb85%2Fctg2%2FvMPz4avEQSf8tKQB29ahDixxtLJQIA%2B5UJRdC%2B2cXB%2FAXllCd8K6Rz0Qn%2BGqhpDMOKyoccGOqUBDnI6zsKonwFAct2LjsmnkCWLggNSEZ7m05a0Vaxshh4wd6Do7arlxTnepIyidGYykhFgCpz0xJegaXKjc55lnbkgwHVM01H68Uch85fo8ldQurmvs0pYRJz8OGU%2FCHYmG1iOiOLu89DCsAd6T%2F3DFh7nPxAP7lgE5CPcWWT5cwksaFm1wpCqXWdW%2FgLCqLFmUG4p84ZCLTGHlHovedM9RDSkQ8KR&X-Amz-Signature=f43f8ab732e548365c7ca6d0fdc696fcec4f5ba7130ca44b41614a46dd8c73c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

