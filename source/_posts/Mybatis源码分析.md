---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN27GLPA%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBmiX%2FJvtU5p1AmilLDZW4P93B8XtkOX2tLdHNwzwf21AiEA968XfSpNq6ohe%2FDtq%2FaIFhA2CazWa%2FF9361%2Fo1WTs20qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMaqT4vqxAwFLeuwAyrcA9Kcddjz4vFGg4BUrmdrzyR28Af4WWpf%2BFwKP5sRpLDXyBKM43qp6FwKIE4OjiMMY0jusxJYh1j%2B4bDR33qo9n0nxG0%2F2hF3ICYsF5TtN0hDENg71B16M42J6C5pSMRuVJ8P%2F7MQ%2Flz0U6nW2BwKhdQBG8l%2BIq1NNkLMaq0wdfftw%2FzcEkyT0EL5ZWIt0htFTTG7EJjH5JJmfvdarxYzLtkQlo9Pw0kiKHiTAaNB89mHzlLves7ZARuZbzfRPo1cy5hoA3kivPLXd%2BX9vSCJSpmahXOwYrvqpNvXPmIDW4YewWtSrm%2FasHMd8yTL9xTFJ0z01Oh68EGgcKvlrOZgBqwZ7MOhIQcRxTq%2BqZDbj1bqvcTUVb0xvdBGA3mT8jie%2BqzRm9uIYaAZ0emQyIG9wNmPa5nMJY7LyjAwkzV2TQQWrk%2Bq6KFYAmVMSKXoejamJU68zNfDSS7AMKk4hrgVcaDnvTV7bT5kfXnYNAECubHNbF6jJctKijUFiNvORpH0g40wbnCH1JpZaFdeo7EZJF56KzJdBZnaFQBGNYgvYh1E0puZrhTxnlFwLQwr7hYr6K4SpIRflMxVcVRQby9nIlIyERc0TW7rihX4UGOj9Kklk2eymBMJ1AHWMZkzMLnmx8cGOqUBzuaahDlMWIUuzoPAXEj1u0xaqqg03O5teN0FqZoV67ahYf8DQSueeacZpZgCacj3XmTJfCIBFl7SCfZ8Gh%2BNeOtGS1j1JDbvyuPFMBHi%2BRw9zqFiTOKcg9Vm9UfANMwnguF3L%2FzB8OySKBSqFBYKkR9%2B0Q7wVyML9hQDEt3A%2BT4QGqdyPBi1BBDTIoFGBJeKpHQO3sIlmIrgr4LBD3poaQJJcdAa&X-Amz-Signature=de1807f84fcd3ba878a00d6b0c77f1242507f0f3f0de47003ff80463a74abc3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

