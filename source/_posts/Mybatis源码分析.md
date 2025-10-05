---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFFVRPQ5%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUImIznjk18P0OLHKvoNLIGL8xZgfGzT%2BOS8iDAOwxVAIhAO3ptfwsGoU6PA4FQ1WpEou6eZL%2FR25AeuIpAhonYoV1Kv8DCHoQABoMNjM3NDIzMTgzODA1IgyxF4QB8XMevybRmHgq3AMOFK4N6BCcmJgovlJmWHNsXEEsM7SKCUDxmgxzP0vRozzYAvTHpN9ZntdOXjgV28De2JqSCQwfRajWlQfPfyN49gJDvdt0mYdZ9BiECqeijiBl9zEN3%2Bd5Zpn4BvB9az2%2Fwsv03XwMsvrb4nizl0xsLBGjRSt%2BbyoChCfWmsSzVbOyEZy5im67ueHYdZfuLifkJM1o1h0M0PUT7kN%2BhrU3w1Kwn8Cx7T%2FmeyXTLuyCu%2FdTk5csCOHpoYoRRTPN8C5EZ8uK5PSQ1C6CqfRvOs%2FCFntdYqiIjfuxJpSOLQlbNXlbZ9hNtzWGyiAqhYkaE00VHhA%2FbJYJpmRLr4BZRCEhDIrEa0Kuuwy%2F2tIdt%2BX5p4xY8H4ZKHvWyUSe7H3dlDmTXrxrPx5%2FEPad98CHG2v2eErHWuoU0JuW%2F1h%2BRzC7QD217Mq0%2BV0reiWS%2FihyP%2BjEan4e7d8r6RFjjniBAKn8k5Q7Qm91H3sa6yLDsMWjFmFhQPZX%2Br6k5CpIhvwmUSIVFtF1FROEOnZ%2BUaAV0bdDa15MWx51vEymn8blkYQjkZcBWNUPi4fCnjfwY%2BikJtLkyyVsZLJLyzQOXlu%2F4RO3blotFp%2BMxcBpE%2BA%2FZe20Zr4ZbBBol2WXWg39xTC%2BwIrHBjqkAT3tiFqvnRYlHIKuX4zNbaDEC%2FL6HVXPMOdgtBvRXJIf%2FsQGRDhSuJSHqbo%2BeHsngLM28wLhbTqzC9RW%2B%2F%2BPq5PcsdJN0RwtMVxOa3aYXAaYkHhRgT55Yylfc1A6VW49SsvejBbSntTkwrlRVidjBoDOblsnBJYIcuGnq%2FHtQrSYszYXhw9DfjstOLH3BLuvx%2BAsbUFyZvRMwKVn8RQ0IsF15pir&X-Amz-Signature=960efb3f66f8483f2a113b0cc6cdfd8be644ba558ed723adc667393cd46750fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

