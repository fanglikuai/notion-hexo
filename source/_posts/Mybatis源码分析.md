---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T4KLVXD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJIMEYCIQCFYsARMgoPgFt9D9IoX7EWzDzlUKTzG1afi0G%2FMmag%2FwIhAOWDhub73RS0iXc7bJGdA6sOMh%2FcIeioML%2F6zun033ouKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzbGhpGP84Bc6KIYPIq3APwrT%2F1rc5bnDSKdrEVj9cvugrqyoiI3x9pVczjQ3ILhbOJXFgpKkX4ozxNauBER4JxW3ar3NKlLuP28CYIot3V%2FOp3rD0uHCoCWQjz6hVUrUeqdIdVQDE55jwnQcZavgdoVwrBcZMRXxdh0JMnnbxH2oaSjKqvt88oW0TEdAg%2Bw1NJt7LMR4XQQhW51T8Wxx9fEji6LE094GqS73gRo1W6TUuXFdf%2FghLM1m7jJJMTBIotl57TclWSEVOgeA0L632gh4oC%2BaenttURwoW83cuEp21MVcMX1v6AX%2Bp%2FBCE9lfjvXo0dzNW8SAdA78kXFNwJRqNW%2FoBNI4PupzRiY4M9AcyM1n11cdW8L5xdF7g9KL%2BKXJnk6lp%2F1jNFLupWWMk5eVJN%2FiNb54ezijt%2BFHmZ9AlifvUarr7qdjP%2B7H3iAiwQ4eg1TBmHFRLdh4O%2BSU%2B13LuRdHx2WPBXs%2Fi8Us4Z97YhzxHzz%2BCHhiYMpZA5%2F57FlYeCwF%2Bcuq80atc2onpEurghBnNZtBf%2FgY5XCy5gz3QbcSMjmSZu3%2FNIfqWS1hov0guZmfrn%2BOdmE5XPjFwn7XK5FBrcyPXfuI4Avm9z6%2F3Kez39%2F8Z9N7KyEZHAgrYqxC1N4qp0bHUw3DC6jNLHBjqkAUN5vyLVqqK0BfaXKNwroIsBJJLxeqqYibGshdxEcXj3kj1GSyVxdlZ8BD8tBrEIrRO7LqvsFxwHq1QBFUzjX2EFcLObeLrO9sTnbTTWKIyCEn3i4ImWBfLw9utgWLAb%2BFtQNcOoKsHfjMC3T1%2FBVB5bW%2F86SwhRd7Rrm1ihPO385lgU9FeCzjt7fcXf2TJ%2F2sMo6j0P5fkFnokyG9FAHHbAH5JV&X-Amz-Signature=b09acb2cb8de84a5c2d8c7d5c1f0f19250f97dde6a1550b69f433eb513fa1c19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

