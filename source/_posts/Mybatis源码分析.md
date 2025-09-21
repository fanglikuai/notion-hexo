---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EEUS7XV%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCzJ450AQHClT6dpcApp7U6QDHdDj8IoU9xd63BtY0pnwIgUBfCLZHKB0pjYUfmzbapgsQkEBsyAjbs2Q2uN%2BxhXqkq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLpNpRu%2BJLeMn7CiZSrcA%2FFGnQgA4%2FtTuhCTmaVW9qPSNsfRWs61dJNdnIUoV0pc0AOD0U7bupiLNo115URzZ%2FclZdgV4g92iGxjFQ7r6jEHynz5dV35iumHWxh0bTTuEeGWluWVO4SQzaUHXve0gqt%2Boajzfh2Ykl94r2nB2CkeOyPpF%2FLzGm3mo4DFzO9bo767EXV6y%2Bd5uzJDFMFsKfdRkp%2BZYVXsVM%2FwutZ%2BurmOEWLYOs3Zlvgrb4T1E2h%2BhFqE8TTYwkQHW4B5iiP%2FbosGtqoIbwC2os6imAoQL1HcJEUceEVKpxsmcaFpffHO3QkLJ3FM2U90MvKgMDII%2BbqfkH8jmsru45GjtmH8T8V%2FZzsarvhx21DV6T6NO%2FXNeqPAXDpbkb%2FykcjXs44teFoIE9aBE6IuuPGF4DtNaSDIrXQuGxECzOyDUIVm3G5gd7ZlnKhLjFwzNKUnKDQgvy7QUJhbFAHlXOnkH2DKVVfT%2FVdhTDO8lxbj7oVI%2F%2BQZ2h43jfpyHWuHak7ibVrEG09Sn3db758dNpx259xBpQ%2FOiWT%2BVTmBah359gN3GVvPDOSYW6L278ZfuPnh7Yd9YYfj06uAHeRbmKyyQ1qbj%2B%2FJYwgyjj5MaTeT6nsCwv2EQ7fmcY4O6vNEBmO0MK2cv8YGOqUBDDpNtfSad13RLNrorvhNU%2Fv8mMuRv1OOjmQNDxJMm0oSMNF5rzeeIZoj8F%2FuP%2F61qHJvUkSTqCxQZisWYIk57v5OuwlqUgna2xkGyzQtXgKX4K2ktDfpJ1x0edvDscaWW%2FPRWhA968BoYqfHlZ9uqzTy6k3hVSGQRjtd65TEkYo8M8nqvoyIwzrLZAJ6Ov850V91pdud6Ee7cPp9cXXbfzcUBZw%2B&X-Amz-Signature=0cd5cf504d096b156669fbe0aef11cbfc5603d338de65d1ea4ca0befd3d89525&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

