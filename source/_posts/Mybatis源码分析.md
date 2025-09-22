---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RD2LLHZ2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDKGrLRalaOc3tj9ioh30c556tIHZfMV1812iOR7tkFOAiBkL2dtPj1ZHXi1JyYJiKhYSLdW1hYqMRbJSWxgYOZ4nyr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMY1GrHbpPh%2Fv7bDppKtwDuiOZrVqRiiAFvwimDaFGUGOsgCCsvFw7amCO6A7OserGIlE4dKXYEctYaxxG8vcx%2Bj7nPybglmkBBW63LSUeNRzh%2BshiGkYtn7mRTKqgFvRX60335ggwIHzMbGbEFecQ4NEgXdlkG6w%2FAE9o2g0crpx5ShAhfs94bVAB4fDKwMS3tQz1N39Sj3ie7rbqifjmx5kUlx52G05RYuN59uWgvf45Zvs3nylYJOiFhU5qDKa%2B0CwWOkkTa4jhK7th1M%2BpuyEOvSCBxQNWv5XbjPe4mVaOaFWdchzpogCg0AiUxE8G0YK8KfrVoaYU03Y8kHulHPC8%2Buo2n3OrOSR7D%2B5My8BNe7Ml1cFyBlNtlFkcbEuB%2B5hbu6KGzyNBRRE3s2m6wxv%2FGqdYKVEXYwOR6IEG9fPRsZHK0DLzB5SjYGIFmuaZ8JimuMsQFtqxNQAzDl5jY1hcOPhb0eXxvvy3HBfMMlZtO21HnET4ZDJj9kOKCC0wChKzkAgHnFYVIitlnpGPC%2BCycrgCEpODIecX21X7Vgx5hxjGzbtmpEvvBXKxWNC7k%2Fn9kNxoLLhGIKd0ui29plH10w8qE%2By6igPHeEW5H%2Bpyi54AsFxfLdKQZeoR0vmf1Rs1arBbHqcb%2FxIwn8rDxgY6pgEj%2BAwgZ5S4%2BIOaetcomcH9jqfgOeh1S2nNVVmEBXQgQtDfpBIr%2F2XBtY5PctZ7N3S7fz16jzkI33czR2MRNP0fx8PWrRrhgsnwIoUx18EzgBNMz%2FYSKyX29MefJ2WK2%2Bn6TDXbLczMy2pgBr75oVAus%2BAzjVThGTdT2YKRUty7V38y0JLzcprBCiMRJKUvaaBO6at1tNDH%2B4dwx3xt1LLNZ36OxLL1&X-Amz-Signature=7ed4dc7cd04a16864e3cdd4ca156868bcd8965d375408dcc876d2425b58a0def&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

