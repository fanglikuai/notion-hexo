---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTF2UMWL%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD0Ct9FZhuqfaBHkXADeJ5k1t%2F%2FdTNAm65GuTWD8yxJnAIhALNjF84O436N%2BTz1hEHfCqZTi4YapY8Uel3OwGM134UMKv8DCEUQABoMNjM3NDIzMTgzODA1IgwR%2FWDAqVP9LFiJrYAq3AMGUpppCeJynkaapZvyLN8DAb3%2FPPWPgXlNCEtygvLY47bZSHI%2FWt1iEkvxl4N9CLwTDaNCgRYLr3BekhXk%2BZiOWqmn2nfiz0WhhCOi4BcrWMrVhxH0fT9zqVeBe6AR8hUh6CXq1%2F%2BJ%2F8ZjMOa68WdKJ8n6aZMqJH3ybEhAZJ8%2F%2B%2BjpIZmdhyZg2MpOyiFBhtaG%2FYw%2BjNcEM6dWZfg78SK%2BzPDATCldManfVLzVo8ciPZLbEtZ5f%2Fnb4wrdSeUmbnMSWvfDrq11JctipSyg%2BB9NB7XQ%2BtzszD2jIEPNi2AipfHrNfgeuWjnq0a5Tn3IdLIfiVVwjMLa19znI1RN0s4YJGM6G7QIEvmUb2z%2Be75mq0t8LTZvXkQevMhCPeDaSy2085bARtGYjt00%2Fa82fP09dgeurP3TDaEk0r%2BVX34EwvDnVx%2B7QPZmfSGf8kKT9bHaYI1ffLLG7LSi3HYs21DYpB84QJ9MofrX3LIStvBUd6SCQ9FEMKe0bchU2jrhWJONFn7%2Fb5WzDFcAzdWCnlxlVebAtdPBhWZWxl%2FnOpcLMtVgypQYJzvROmJFpMtMLr7uViDp9bWtH1aTqnDYVHPH6zO%2F4nlRnDv8sBO8TIslzrQXcg%2F8nFbJnfqmPTD08P7GBjqkAY4ESHnwtZ0cok9jEwE41IQmTpuVGcMFQCkRXPS8XAiN7hnHQmyxXsY1g5nurv%2B%2BteRMtMvyPN%2BCr219EGknAKy%2FbLXco2RFNx1LLiBYnh02IWctPqq35R4Lh59yH9qV3u5%2B9N8yMJYqZQrFonEou8TElgevQMpz4N40GT1e9HOymazCOp3HVjucLubEiuAcdoLo5DZtOlkXtJnBQRMiI4JJ9n9Y&X-Amz-Signature=dbfa51d5ee3a0d07d4a57860adf8ad7e6c353192ba41ee410704ea0f2ad9f86d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

