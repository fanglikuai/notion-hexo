---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PAFPKIZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFXh55A4nlPYVtmZW14iqNcIqE10Vcoqiox%2F0Vy6HanaAiEA7zxFGd6FOK2Q6bTsnq%2FpWE3lBs0vN04wO0C0C9pM7eYq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDMrE5NvOUKYgWuSDoSrcAx%2BfuNTk13buDrCbgiQC8jTiqSv1LK6DowCDivSqbPXqQyYoP8LV2QB5rclOPU2581qqkWi4b%2BNTcU8qpD%2BFHLdJR8UOCzW8bVzoQ0ag1RfwrEi7ol%2F1c0EhgsMWWLUndiUbOthbDwlVd5DUGL1GLs%2B9lP7Y0YhDpEakam1oq%2FdxsJRaMYLqQppXVXdqO6ibNQE23OvAQOlUCtlUeVFHfl6dhq4Vj1VDLX57%2BAZPJGzjUYkD3w6SJqFYWWti5W1ngzdf2Lc0qnOETFUtkbaOLzS0HIWBtRQwNF8r0DY4O1QccGTsksv8BF%2B5urpniDLxAxHnuelMyRNJ37qorsnhSjwwIANpZxfkO6U9MZOHF%2FEdGkaFr9YC0axDOfmHEgyuG3rDjAF2RHOtS5Vz%2FdxMSnpilE9HFfRcczn75ePIJETj3USI4MtKXVCEb%2FkHkP1HHcVDdSMB3l8ebiYvwXlSEhJXwUYY8lxBMO3jUNWCiXNf%2BLswTp2inAQapr09zyeTsYkKcNBqHy1KMz6l%2BMwnHSvqylJffQjV9LSkWZmUsGwY3eNkO6kWb4TQjt2pDlNz4cAIUS7lUKLOhNxyBU1ifwywU5cZJNPmr404bWZ%2FpahMp1AeetnUTm5MP5HLML7d3MgGOqUBsCvBmMogOCYinJWApmwsnZRFxBFEY%2Bes793CkmmAryXXE6eQ%2FOyeKpOoA9QcCycYd9AgXa%2FvOpQnN4LUlS1Adjmvc5KxNVN0ZRwXlHqtI4QQCXpUD1YhZWa1Xw4StRUVGlXq32BURKMwFaad3rvlEiK1ySHc1kyvddUyTZy8aRdPj65hOz610eeM7thrX8Du7%2BNUSGvcXNSADdiktYbUi5ZWyouU&X-Amz-Signature=bb8281a538a99718531d71ca2d78b701ef9b0dd37fd84a763d79e14651777dc9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

