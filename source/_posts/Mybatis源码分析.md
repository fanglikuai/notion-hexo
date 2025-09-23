---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EBLQOYT%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T140044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCJ6tL1QI8jf0rAFOCVahXeLpJbLIfEyzDLgUf4e8JmlgIgLfkadmb1f0ONqwakfwZyVR0nyf%2F617kIzPdOddOFXK4q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDGN%2FWRzb0ssqUwdJmircA7%2FpFtman7k2kTSPW7r2SMfn98Ol5yVp64WEtH3TY8zXnau%2FNxUTJcMsf5LfWdyNj%2FpooNq5NSK9Q7tgIWUifWkMNXynMP2g3r%2Ftb40yECBsCzpao4CXGxhJKZ6wer6gOMEgaaR0VnGcO5n8E%2BffRq9CNZPLH9jBwQLPLy0NXN1pYyMn4aPXRKQ6b%2FGXHr%2FyXbSPcHWpVMBsc4wcqhISOX%2BcdAfbPySVjb7eUYijnPXK23RHurzwzC%2B4d6Cm4dKmBfHhrZxGCRjMEFks7wLEHez%2B3zCYsCkccGOOTuNeUoiDiPzJ0Zyt9jSpxWovaRVEQ6h%2BiE%2BvRGei05IDjq%2F6aUdoCq6zBdTeLxbPn2FFZWoRSY3r%2Fsz5X%2Bw4xg32hCZa9fK8y8r3zV%2FF3zRTFSqBcZtwQ9jnJlDgsRE%2BTqLU%2BW45PUF%2B%2BqJsjNEQNeTMwtKXoiQv%2FEnWDZ7rjdo8e2Sff8v3H%2F%2BmbIiHa3panec9yawc9r%2BlOk3xb7U7eBIXI19RvhPQZtlug6R7dMSMD%2B2UdJ8jUrKSl66rMTTDsaEWVXFl1NAV1hLr9x3gmxOkt52Khx3AP3%2B2Wjiu2cQNUY1ks5b8S40djKUFHPWUd%2ByWb9tIcSCENRRq0%2BklNrqCMJeyysYGOqUBcKJ5ERHWlV1kcFkn28qJZ0nPOhj5dalJVxa5%2BNyG6X6vcQrf1N114E%2F9L0VF4hVLN2qluPPfVdHY7nFnGl3bY8OKkk8B8bBfG2FvgucWVQs4bYujFnz43C6msLScbFIdLM1aUPm2BSJOhI5sJnC9Q7hEr2%2F8gQNQ8wl1Yz2BJqs%2FvrWAwmh5OQN6VRe1r%2BSkrS65OlCKvfyRWK9nYVMRBupvXMRn&X-Amz-Signature=69cf4bbd9da199fef721615d27042b937378fd1c51dd4af7e1925dfd27d77781&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

