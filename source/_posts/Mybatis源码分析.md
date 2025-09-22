---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWGV3OFJ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSyF4ZtSH2%2FQuRDOtNiygXtQCagiYPQGuCTjIYhkDkcgIgSbhYLI%2F2AOl%2B32c%2FOP4RHGLZ3JP2YHzsGE4%2Fo98OOEEq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDPtYD4E5b5Zkm0Yr%2FircAxoXlB5COHrbZr%2F073gmXVYtWYz8ckqusx1xpa43VbXnB9jc4MwaTVoeEn%2B%2F6J2%2BDLE7iEqAUSoxrzVFqlUStEW3qb%2FxkisiRSq908qzbiM4ycihIc5nxUvrtjGOZ1l0Q28gbxfesT5YP5Z4wRPX%2FTKPssq9KWQIhLGWlgSokeM3uCU9H%2BAz2OD%2FJfbSD6rIeSjpDfaprzw%2BOvrI0K3uDrd0PiMx4i088uTZl8xUb0311VdZQnqaXJ19pZf9uPgCPjnat4UclVl%2FtHaU%2BrBzh9lM6Yyvvf1KyOxR2tyTVE0x5YowifpnpCKsKjHZDNALHuNguAHcouHE6zADM%2BIesJq%2BwENE3JTuZa8RZwotcNr7Ph7YTRnSdifWZ9a2BSYIvSfeEN0yolStHlLOoIsqmiEGMKCxFeuWrmHBEQgtxsEWjw2gYXvLMI%2B4R2UDiJYf9T5ZpzBpgVSNnuJzQ%2FcXPjRrcHAJq%2BhWF1bwyBuWWoYr6GxO0GmC68phtqbZUqG8I926L8k2HIFYYqxBD4G5v%2F6wMiwhWF3hGUbN6Q0kQrNEdqqjkFRvuUL%2Fow1hQ%2F3tYJZkwTqDU3UtMFnSTooF5dOg3BEzO1%2BWHd%2F89GzIH7gkrh5NRB6VnZPV6O24MK%2BwwsYGOqUBLCQZ9Wa5rEQq4qHxgP5cHpeQx3Wy3so6hFCEEKWyPvQXCEBPi74%2BAOa2gPJVglG5xWd3Kbxgc3SudH1phC3o23cok9QBtQUJl%2Fd5xvTRVYWtVZBi4%2BkiRfjtY3E0C5iIgkfpC8ULShEmhNMJqIB%2B6d9hbR95XnE5ecIwxggQs8elZN1%2BdHlzlwJZ8NiXwEm7PfhN%2F5ef5T2esSw7ZWZfHcyA0USW&X-Amz-Signature=45b8ec242cf056bbbf99563e1945f79d8f587cf42a4b6e6140362d2e27b1052f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

