---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XPX5PEK6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEnO5LB9U83joC2%2BRZEl8qigAASakmbCZvYvrnozAN49AiBdLViB4G4iXBsXhMBPSscE3rucF3XGl6jTZED885gRqir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMRbJlw3FytIx%2BN%2BssKtwD5cjwWrAPCsJoWhmtDo%2FNux5votYCCfA5nMyWflbZtci4HfwgUPazi%2BVuzp4d%2Bzsf5BS3e49BGh61YpvbHjoretmqhjjjP%2FUyBpIl%2Fuc%2B%2FhW275vq9eA9iy6KHSHKi%2Fswh%2FCsge5OUU74lLqtfWbwOSb7RgTmUFzXq9cjajEMYHm%2B6xBdQoypGbWsdOShqMO8%2F%2BXEbh82rgydJgD3T4cus2tnNUcaMFxfU6hJ9XSMMAFl8LQQJmFjZ2y5JeMZ2%2FFa3dUFk0HCyfyJZIf0YOr2wTVn%2BrNhLU%2FhircuaVuMWSOVkf5v%2BNdtMgLjM7No5pRSSh%2FNT1hEfBsD7PCMVMbYCuAFwReOkeeeQQrqRYlhMx3mRKnVAGWua4uNNoHHLdW%2BwL3%2FUclz8Duyr5dCxn2ZX7YjAMcLXnTLf6gYP%2Fvy%2BWj1ucCDzrwThivXh1ixwM%2FEL98QhSQlFXXbGPNTVtPn6ylG1DIzmTj5hEAnzjwRd%2B5ENrBaQF%2FWABOGZM4JLb3ZDKmHvUnBCikwxhSS9RdqkKu5dHDfyEf2Tn8ToP7gjEJpQ7PdrqTLN9ZVhRbv3SCDxF88dxi22UgvbRC24sTyevdrs2fWRbETGbuj2QyMBHVSWCsoaZDf6i4oPqkwt5CFxwY6pgFUceBT8WJQar6HCM3%2BBaYLFUVC3A0UhonSdWAk4HZmDseHsd1qVeCJ8hld6CCZh5eccSzfBrluWkBKctcQUZFFi8KWeNKxSpMILwa8nC%2BUYWMIB5loiyS1RhqKBKiJPXJwEqJpozUo0zkHTFDJMqTMKcjh%2FN%2Bni4IeU9PByz2SAN6yV5GWUOKB5A9P0wJJCMMokVNR98PMNlwElwug7rZcdEf%2B2V9U&X-Amz-Signature=1bbb6e1e2982b5d817144a1ee10d9a3566baf37521c947262f26e4d80523b297&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

