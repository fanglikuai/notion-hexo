---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJ2IYCSN%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCVKxs0r8hC0ZCukrErO8fVwd0mzGF%2FN3ar%2FnwYveJ7tQIgQlGcwN4X8Fi2jic%2BWcQ2eNW1fxhnlsDPfQtIM7Rdl0gq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDERpWExtJ0bYH8XxlircAyQdjq%2FZ8OXsSIYhyjy0S9K%2BZ69dIoUW%2BRvQt1Slw27Sl7RhOL6GxPXYLiETjX64ixHnVoiBr5aw%2Be25G%2BtGmXST7m6D%2BRGlgct2mxrrFOkINtI5lWdyniOdLfqVi60Rbupkbi1LVguz5EzeY9hBzNFdFv%2B1xZLY4R%2Fv53cGw5vbWP%2FNYGEhbX26JQX33%2BgTOWrx9CCTi39DYgVM9%2BjDqcpDgIdsKQQDRqDTxA5aD6nJcnEFaVUphyQAN7Hmcl6B255s93q37ETItrEPTkuXy1GvdrVli4sKi4xSw3QobBLqDJ3m%2BOLEuUjcyoftzd1sQzaLCrBaqGq2kB1j5bV233ozmkfod8UH31F3wiss9OnodtKSf2im767qsJz1ruayJm%2FjeL9Z0YVJlUKa5dTN6QC2uFE5IsZPaQ5Jl5eo%2F98prNM4fSmhTeSe%2FaotZr%2FT6JmErm88sDMo8i7oLVB58gS2qB5taUrCJR4ca4JiGBf6HIfx6TU3sYE15jkcf7mrMf9F0G67wDO5ChPSGWAeNMrlys9Cgft2rsw7kv51Odwv34RkumWVMfYhXzt%2BH6V1CeDE7BRhGGyJHVFipZVQ3vgGIO8n2%2Fro9jmPeYVbgFGiDa7p%2BC6bX9fwQJXQMJ700MgGOqUBfEDh8t6kTBcwZlYID%2FhvVFYA8Tgz2Qc3dFNcBw%2F7BAddl%2B3ReQ49eydsLOJbUoKE1kGks3IV%2FCR1JkG4peK3%2FdLydH4lie%2B4bSMLc3HD6jSHVwWyI1uiwscO%2FFon40ejiOYfnoeHUQCzuXPqKl7vQNGJFp7bgWMIbNfmqH3jZCOIq7w1Heoc51i%2BzkjWwk7V%2BP8aColYo3h0%2BykzFEyUik69WuQy&X-Amz-Signature=cb4c90bae7c05772b541a58cec8c6f767f64640cf526e28b6fcccef244f658c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

