---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q243JJVY%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIFCahMuBUkOaCokFYU424ZMcy3yos2Rj9ups%2FPgCqdjYAiEApi5aUqT5GBYC7Y1Rmeqp1QBGzylxVhxVTKUUamhGGYQqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC6GkA9aT1OGTY47FyrcA23BsUd3z6lcXdbyv0neHi8eA1WiV9XUJyL6ML%2BRCY66bgJt%2BxTwEl%2BmAKPMKbRwouJDQtgUEPdM%2Fl06BBZU5M4tIVTyaFxIryz48NcMiq7EyIWm0S1bIbjbap6PtxoZkGxpyU2dcFuwxA9S70Z%2Fd%2B6W7cTD%2F4qBSTYUuWre5i60oUt9NSDPp1S3%2BiUQ1HjWPXdwnPoW5Omm4nyh4eRF3K%2BT7zXr3tq80lmJK%2FdimMhWZSo7HePgVXRzDrfVsftrczQajXbXtgBuBqwJNt7eY5MKjh%2BoJMmu2PX%2B%2FT%2BejFRi7OTlotkuh9d%2FwQa4j%2FNTIJIeKyDTrvy3mMm7ILa3baxeMkG3kVN3uM4d6zbbIDxsy6cOF78pk6R2V0%2FnT8gbv%2FAukHx9SS8lgt2VmQPCKT7yHdBFkbeZHCtHtMpWrjpgBSoIK0%2BOdESfYugIewFAui7fhp3dC5E10T2d1e5ovK6phIWUZz6I4xMutXEp6TWd0L%2BhxBT6EJH%2Bz02tgn6onOh9HOgXnYMdUfB%2BGQxT1ur6whd6DiP6w0DdX9b4%2BFSu5wL3JYfJaBCS6i52Ts9pZUWri7rdG5ohdNZWk6kwpriFlYVaZ0%2BNxAwHXdxCRlzeDSM%2BBQIyI6SePPBvMMnQlscGOqUBX5puYZ%2BsAx5JUnctTdKYfGKwukBdP8lAVAoIwqLXKjg4hdE44TH%2FmVaOfOa78X3ByS6%2FlUlARW9oLBqnpCSutCglJ5EsiJKCAozP8P45uaXsUP9cx7OQYxpVqM68IFx3%2Bi3jdIeCwRFVQE1yO3BRneZJipOu6XEft5S8ph3KbCFTfmvjprhpL8GCpap35um9Fx7Qv138kfZZYLTo%2BSCFqqi%2BObEY&X-Amz-Signature=30607d783544af43b02f07696e7b55e1d1e9f5f23868bc7da41813cbcd9b378d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

