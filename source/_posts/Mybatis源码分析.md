---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYB7VECC%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDq2z4cWVqQ6y%2Bzk%2B83Xw3aZHZaRVHnijbBdYBxoTXLcgIhAOrWi6zd3FFY2Hf7Jyo8GSjxc6E1ScQMuTvFcUXUPf5UKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp7CeMdKLXo9kizfUq3AOeS5YF1oinqU%2BZtLs33kNBmPhFfdUkwZJ7MRGppPasYOIVVkr41seqyHmgyZlQAbDikOw7T0floWKrfZypsUVZNVT0aqqUQsvMq9UiV4FNRXmdNtBTRFTUa6Z51bmAfc7%2BaVFSeYQduQH7gL4nD39qKVAWYwkMzlVH3UN2RnlToyerRjMF4nWu7m2FgZfx3TiLxt8YBwWSTLVbAZy%2F%2BPJzRLt0rGkGE6iuiAvEvW1MS2Z7U3M7YxS%2BzE%2Bm8AbzN2px0iGDDvUiLX2Yk8xFjb5P3sHmt7%2Fx1jBbJ%2BnY%2BIqOXpIKDsCjIcyek5Uamnq8tC8IUI3t%2F1afbW5NTC9wibC3XLpmBy3IIyiagem35Ug%2BLyg32koSryzG3SOHSEOIs%2Fke8LNqH2Vd5Fsiiu750K2%2BFcevTTK83OMG0PWn%2FXTzyPmPfT1gphEM2E7qFUPL%2FWpwA2DbqYKpENnAGOGtKYd0Y2LnO5JyCYburfpUd1RRM%2FEshQrzd9TDL%2FCYn2cn4mEPfL0O2StijSgMRa3kFI3I43m3OaA4hE2N%2FMttBrTkTdXi%2FBNs%2BJz4KG5LLacZQ%2FJzwdOICzSA0oaTdgeBUuuoHi4fR6ZKGAXSwcUA%2Fgpytk7IMo%2FOayzDlMfbhDChhYbIBjqkAbNzxn%2BGo7dJYHI5vOXdj6gpx7bbYhjtTf2SkQMrZnj7ESwvAXECcuAtoPnmsabaRof7gCggNZFiEcRFd%2B5KxMAq%2BihzThSpg7mDqZR4CE8bC78GVCpK%2FUrwidUvbmL73O8I2PMHBcM%2BPw40ampJ0TOoEk%2FoJ3J3eUx%2B7YMSyEcy745LWOrD2ARQZl%2BsOdlfYKymrzCf3cT5HurlhZgd0EEvVNKI&X-Amz-Signature=d14b347f1294ad90a7e9e51a61cec8d88b01aa59e84e723e62b54ee9052edae2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

