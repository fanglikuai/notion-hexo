---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YABPNZSF%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDJkYWLuvb53uIESQhbhE55w6sVYxoIH1QjWHaNqTcYzgIgSWhi58a9bSUMnbVWJdE1vfHMRq33k%2B3NgUW8XPdLKKgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMASSMhTkldupvZsQCrcA2UjQvKwI3bbCiv%2B49djzJmdipMcbMImvkFF%2FZS6MQs0e9PHhHE3r3Z9tnFYU0qfA9QwiZQQ4MelZJKl%2B4%2Bw1z0LMVoICt%2FChaqr%2FyR8i9hQtrmX6b89McF816i6eyU%2F2d6WGaTar1dUGSMDWSf8Cxjn9D%2BToAr4ZqJgLHsi107zlq9aQOuEblya91b1nP9wn5wMQababF9qDTPMsQNHNSyvOT0fZL%2BsoO28qCQ2s7FTO1rrbqrCjEMVNR7gJ8LR0Y9OonKZHRJ5vp0hMdLLG2TDsTmImHrQYkvEwDn8%2FjSmL6Se5nALD3O0IPC%2FIVfKBFQAMzQxq5C76%2B5NdAo3jE1DkYpp75CvddpP1HP0PjThODHuwQeL0R4HSwMXURhG1QHMaOUyIaMu3YywO9cslXBGZi7eywpxxNZZOPCTfKsfU0k1ECI6XA38saQgFA15YRlsDwP95KUFCm%2FcgO%2FP7U34SZK8LjihQsMooYlSuyfvvHIN66OqrddfMTdDtVBzN62b6T0gNIcIUxc3a5j3lGrVNSsJ8ga1kqdRojAvm7rNEQCnT08E%2F2mRTYlrsaHwHcSrrrqBpRoEfyCntVN75SKad9TMUlldeISi4kWAB%2BzrXsHZlCGtYlL4jP8GMKOThcgGOqUBmU%2BO5xt1ArCkh08hAaTAaK%2Bs94tf%2FlkXzG%2BSHcFSR4KMWm0%2B2beXYb0NOJerSgyc5xr0AoxPRCqgkfZd5zbeKMnpQqA02pwBNiLVNFgG5me0pSxgqWK7kQVLIPMjcFK%2F9C4M%2B9JcIlmgRKdI1eNvL0xEVeJLRnIQBbBYe%2BqlENdO8M7RKiv1G4H4oS1VK503s4quF%2FoWNb2dcIGe4zGfPdi1h5hF&X-Amz-Signature=e3a2ef8c33b989fbae809e385fab1285b41cc2d28770a19af434fbdbbb8fd16e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

