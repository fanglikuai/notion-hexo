---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOCHLPRX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfZK%2FfD%2BVT3cLvUOmeHFccVv5Gm3WjWn1LGtNKaShOPgIgLVxiOOzaKO9GvRjlx4npWLlzlC0vn%2BIWa660hcBtzZ4q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDIYQevxYwZX0y9G%2F6ircAw7VfwKSpc8ssmc9tWcmGsXbjsK65bHDp4cgrGyir8x2C1u5rQpJMqj6K3X64fiYcBrnvAOL5ESfSK7jgaZp4sM0R%2BWhJfyyfgdXSao24kPbUDKTanLxuYlb7BJjyoejqY%2BKjm1qOYHGJayZKSXLz2BxpdkdWlNVa1%2BpYoTpXXpWLEjLgb5ft40TEMb6vdp2KZfkYdfggWE18KG5kShcmCKl2ON6Wr6Rdf%2Bflni%2BvHuf9GWgem9H7qvm%2FYPKJMvioUXdng6zEssKc66QCHf%2FvgLign6NRbBJHAPdcGLsci7YOacGcnSnJiQxcWSIZVrMFxJ50HZ9jX3RO%2FrKXUGkqvPDYbETuTcDwJ4kLuIcPUtsZpabNxlquzq4NHSzIIyUGq%2BlkFraa4plJhpaU9bAA67brOlnobmi%2FTQqK53YFL1VV8%2FcG25p0zsg4V7G6ZJUIDUobi5kwLSvsL5CAFemykO%2FfyLsPBATJd6lLKfT%2BreGvjbdgR9zE95vvNcs4r5OqVJIDiYRmrONNT5609ZtVVm%2B2u3tngA9%2FOWPEAeMhJW4PBwpoVOKLaLb3PIol7GB15lvlWY%2FaUdlnS9QmV7z16ymPV8ewZrmwFjVnSuQAECWtuR6RYf7cvaVoQLBMPLfwcYGOqUBWAvUc8YaD%2Bd8%2FFFiJICAbyOV18lbNiB0PU8ylHtwQIvG7zdyTdX7epHnphd6Dad%2BLcMcYWCsIdS8BdooT2m75GFYWmHauU6Y5sRB%2FrNuchmtb2nbDIGc6bNvBMdDRhpP3Pu66i%2FVSpNYpg%2BGDlKpXM4SFqn3nbGU5d1F6BLMFlHv8U136tzTHIgwTOf6dz3CUAyKIsnSGlTgWwLrEf5mNQEl3cxS&X-Amz-Signature=3da95879aadd45951044a1917b6606295a5c0c15b919f546fc75929144e840f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

