---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TM2EOCV3%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXrBBN19h%2Fg1U6SNpOA%2B0%2F6i1VZAzrbdZ2fYvhq1ahngIgD8w0Wlll9YQijW%2BURp1S4eV6lILKU7Bg4UhhdZfV770q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDFRkm0CIRvXN0OhIvircA6hyNn92wizzhK4WfpzNo4ax5UeIuUtI15ZXEOZIRWc4%2BUgO%2FNWZI%2BBGvDMWgqIUFgHDxg2SPGLekklovoDO2ABKR0uPscapulPGvCoQtEEL0nodD0nvJESRwD8FSW%2BiB1WNBt%2FPz0klcaEOfcrsOBnULrYnwiJ2cra1LZ4TnIy%2FS3u9PTCTCFud%2FyfeQVsXgcnOIM0AMyVs1WC3GHFgEQnKOCk59xZzOc5MuSnjd1e2xU9QVApKcr9j3dmTaNDGkSPZSWr9NwyZZR3BxlVjtZhwLt8OISOBi2ui0bCYQKXlNtvmcVyVdy1JiqVSs0%2Fr3B1F6zfkRFp%2FhFewos1kOJR4%2F0gNZCFETcl%2BYELataxBtRfw2DxmD7uCVv2EUpYIuSQKYT8%2Fzf8bzKeF1aadhJElvdOAh%2FOYLiZhUuXItUPmVBJuhso%2BUYdSHDJgTyBGRJR0nw4RxlM9ffRY93B6EWIFeIFDLO0hVQhq4JtfdymJDxYcv1IhO0eyD70%2B%2B7mPZwKl4IfWnUgW3Dg40uWYdgrnpBgIuCVb3%2BypCJQA82%2FTQWFK9onMoJ2kp%2Figx%2FOQPeyMLtWTOBc%2BOfjZaUfwu2ynA5NYa9gN1GoTcFx%2FwcnoouGdYKwTWcijYm7GMLyzz8YGOqUBRJfeJedOXpHp1puwIYAzRmZWETl5DrdCn%2BXbpkDgVqkB7nQeL%2F2uzVnK%2FEgPUzo%2FFoguKbZ%2B0cUpr4VplCNHcxC%2BVd9ogzyKiTwXj70zsaQZVV4AVC%2FAVaADhzPbZ7lZbuOAJEe7xf3eSRd%2F7dQ7FWOfT1cu2jUvnIsY70Xgn85neUXojrCE%2BOrdwaCOT3OapmyyfO1RwlRYtpWqeKMDvMPOLRDD&X-Amz-Signature=62c909bf098dc72ef2b1c4e0e6a43bffde5cfea756cfb5a2c7802a6181c379c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

