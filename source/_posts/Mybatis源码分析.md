---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWJB6M3S%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJIMEYCIQD2EqmzMyOv9oq%2FDACajNnPlyx5ZsNeU3bJ%2BrLQPku7xwIhAOWWcK9u95rTOLrk1Q2%2FVXQoTkbdegfhkXipz5Wt0vhXKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igya2rFdZ2oUd9R%2BG84q3AOHUdPBSKjoMDfXenMHzgPoQTXbzl0KqHbGL2TNt6D6rld%2FoOTlWVyJAn3dgo%2BFB0WWy04VXiniLdT3KNHof%2FDOWqq1o9KQyYdT4vbFSE5pcS2OKbw2RDcVrFRP8vUM1LstYDHdxWrTqupnLlvcC%2FbnZyELTTvDrPuc%2Fd%2FaPh5w%2FSZQTG192BuEfRkwfUViGttvzpX7KyIlsyYNt7PGdWpEHAmhIN4Q7s4950zKsgVgAWkYN6ReFkzvvZvNXOpxHKe1AnwK7uzu3tajNHQf9UvwHw%2BbOfHC0PtXAEosuXovlioX4WOdPxBKJFDCSCKkJMVQR32Me%2B0EHcgOm5bgiMYKy5sq9EIQuR2%2Bm%2FFoYyalsKXUBgOouAbRTPqeNerRDqApAzTXuodioBGOODeuQi2GsicgjU0K%2Bo40nKZLPSG%2BA96MSZn4pvOdtXe7E8f57SEi3ornsKh715LJIAqx%2FUPWuEai2WsvNukOmR77%2BmRDNHRx6MCVtFKCNSo73M6XQeOVOY0L%2FUv40Fdn%2BHo9IavJ5ay2IlXEEnUzFEE7sG3gPcG5t%2F7v%2FrWlQXO3X5i3P%2B2axQW%2F6Za6CqANP9iOZ9lVuvPr50zwOP2nTZEHub3Ile6qOqfDK7NayAC2jTCbk5DIBjqkAQs727pKWA9OmlE8UFv4MTSDq36lQ6t2hMJimqs%2BMaeUBj1Aa%2BbCPZ%2FXiON8Q3qsUfeZ%2FCRsWzEMqStnrPXOSrjJEwP8kxLyuArwkPUDnmAQ9PKRbuYxc6ogeDdIcnlNZKQhL4NS7M8PKV9W5MX73hEuBHNa8EdSfWsJNhtFsajf3%2F%2BfYxaAL8kvJWJhnHbTsecwCqs1m3jW%2B1ko4NXLLp5QnKbc&X-Amz-Signature=a1786fd5eec2f9193dae61cb71756773632956bd12c9b7d9029fa69a047d958d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

