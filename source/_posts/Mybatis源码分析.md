---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDBANFEY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDiyAuXe9AIZVHoMgY%2BL7KUBmeS4Z0r0c7Dt6ppC1DFmQIhAJD%2B9pzaVXglPw5PC%2FFCgMtxhoG0%2FMQP2FETT1HuYlxmKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHqkNV%2B0xaLW6ItrYq3AOJNZ3uQLGqvSrJKp7o7nsyLRQ3fS47fDr%2BV5hVM1hDrkQHKdFV0jyGJUnrPZup6%2BohsAngUnIKN8hP2PPqHHlUtdQbk9tKe39eoOc0BXj%2BINJUuhUULxciKbsPE0DEbVGOBSKhvNWXJ00Qtet2JoH4jcwXRdwvSozHC6DmOcF0pObn45%2FTrsXppavnVk09Y2TYTZcOznt%2F5OL%2B2slhsjBo3kG2vPCoE4OxIWfo%2B0HowvYEewcEE2OAEEgooVD695CHdDjWlDsh883vn1KQPdwSXpf6l8myMjawWLcI8MFC4bNXVum3Gcp3D3%2Biju5%2BgR0dDhinHVTY0kzAPMedqEiH9H26MVL7bowUIPiY0yaTq34gCI1DaVslD9XZDN%2BQCG4r%2FLLCNBxaS3P%2Bu%2B%2Fv033pnhZEaXW6keIpmIZcJriABUWXtOLtHrRgezyqUx0jqp%2FJlZQ7qYM0qXLO2VekXKsVMkvL7PXw0kVsYIcBsAPXJAn2np2MLvi49axRqddyNIKpwEH6Z%2BW%2FxaDCgQFnrWGO1F95hsuIPuV4pXvrUJV7C3wjj5fpRJlswBdLGjhVLEvdwj3XUoxKruX4nsZgIsfQGMh1kVnlDvLaiMG4ZxnWNWotRDKsL0f%2BvcjpDzDPgMPIBjqkAejCF4iD%2BwJQ%2FJsV8iAC7XKO%2Fj9cEdXAb4t3krfYN%2F%2Bz2zxWV5XrljwxKCRYHei%2BL04SaulRPeWZYukt8zSypkbVXqe%2FLPTg6iJfzRFYTqZvgYrnNP0hXnVwV6JmPvDuLuZ8UTZ7Ihy5uecVNz%2BnjbWD2LjaLH35bbabck48fHhJeHYgae4TwePnzQfguM%2Fb2jczOk6RUUQrI5YERdwPdeNCnVw2&X-Amz-Signature=fa15652244772b87b08ef4033c43d8280740673984d6747712b968b41624702a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

