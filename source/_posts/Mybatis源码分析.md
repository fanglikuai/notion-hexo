---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIFQCEQL%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIQDAyKWJ84JBJRdAqdtsgF2oxx1BB5HIIAuSoLXuTDvFcAIgffjbjbfADQI7sV0WXCRWystKewRE09lMaWSd%2F%2F0Y248qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFOESXv3sbEmtbWZkyrcA6DUVfVE9TGd0l%2BD3ZLXBGISiI2az0DlQnsPzNyk36v%2FBAdR%2FXyKSnqovcCVrg6cA2nAtKqk%2BEKtZ8lKbJVIUee7HsmH6QoyHY7NYt88pvy%2B%2FIFvGtENAlDBydXbWrt628QK4OnwjbQhnWgK5BX1tSOyDcwEgeQS5j2usNAevJEyIx4mHFlLDaQHmRUzaT9hTm1i0M9SjNQnV6ehCIfcDO4ELrnzecgttwPxq4JI34hK8iweBC7SQRhCnibpVU20ypLD8oJ2gXIzRMZd8nKNV7rbVdE8HXMXJ7mn%2BKSxgLezHdDFLxWj7n%2BIjXNtKdW8%2Bql7EXbcB8frH8cRmua8IzYBOnzzW2DfwCrudAG17PJBsHO58YFymsx4Dg0fazifhcHsrlpztArm3sSLpbopgPyoKLmuYbF8iLoAtC4KIKn79gB%2BOnWJAkEd040Hx4cZ%2B40MoSN9g9q02gRg%2FhDgmRRR7LZ9hrmI6wym%2B%2BhgHmcbfz44DqHZUrmxSvqX43Piq4NndUDliezBMsDpfW%2Fy4EzzqZfN7L54jTezfS%2BJ11ii0bKs%2FoT4CiqyiViUfanByLdgpNY1DKN0Hqa9X0C%2FiAUFFtAf6v2kioyFXsH4Wx6igX69s8TYMGaTgtyPMJjKkMgGOqUBAYrLC81yhwttkWk68vZXMo1M3RnF99Airupsr6fkP8RmvCLApk0%2Ba356dLM0HU%2FhM%2BwJ79LrrUhrtVM1d8UZvW5guKAgMt7CMDCh0QW9YJLzdX9R8nYnGBOyjYU0pud%2BsSlXxMGRlugEEX1HRpnem7xMoph63w%2FW1t6LguswsSbTttj9gseqLcJVbseMBZuGKk58uRwTPaH7oU%2F5oidvF58ch29V&X-Amz-Signature=222424c163a5a0b19f58b3ddea9587a2759e07602937b3113769ffce372bb2ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

