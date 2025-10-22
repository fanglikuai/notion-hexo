---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP2BOGOM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCr2hrq78unOVCMbKQXtdrf31E4z%2FF9lWoJZPp1%2B0rv6QIhALqOU%2Bzrfj7ZYU4Z6iYg%2FksDRWRHawZVuf3nC67w2qIkKv8DCCoQABoMNjM3NDIzMTgzODA1IgwoC4sOuaC2tulSHBAq3APnZmuCdgGu4a7UBWq4EODtHeYew4IG7qhjowOwU9QKQggKA%2F2Q2JT1f0rf2QPtDeayE8bW4mv%2FuFy%2FSyVxBfgQ3DL8qkO8mjXlNpuUqSAxVMl3ltbPOSDMQpprDEszfv4%2BCIctRa8FvPVD9wJqFi6yDJDunRQe223XBlUTm8R0ww1QDRI262b2Zg7CLknuz2O6Gk%2F4r7dw%2Br2%2FEx9zW3YyxOFvIMPvmcQB7KTfPn8C%2FTikqmfiORjtjfooHmAql9AiWWrvYi4v0d1Z9Oh1dwxh%2Bk7rX%2FXaXXnEJvuzUAT7Cqr1EfpiahBQMgp620Vplfu%2BmpVOGE0qcDkJOnGBPOMN3tCDSlAE48nPmRbpq1x2B1Xb%2BBRh50xhR7u4yYW3WQP%2FDT1Au0JwTD%2F6WO7KcmivE7Law%2BFj3SG%2FCHYxnn0WE%2B323JTSMbNzPX37giJYTIwB16yti22Ttr8SM%2BCe%2FcRB4DgiCo6ixgtQyAQwCg8aD3sG4sABab7NRCfQwlpR8Ipoi%2FiIvoz2YOc4eO%2FqVBnESCp%2FXZagIpUWj%2BTiIdIwB3eXkBoOxt1Gv%2BALu0WRbpnY5K1aZOaUXw5xM77x7585HSeTacxRZkDP%2F%2F5QXPF16d7Y5UXswRqYJ51L8jDauOLHBjqkATYLNiHqRyWoCO%2F%2F8VfwgwTiaNPCpVlNAIIwtXPx7WH0Yr92XEEpUrV9XdoKxO2G6C6B6dZ5Pk4FXD6a%2B5k2YjiCHBwvpc7k1F08zkBuAYRm74rwIGdMkmaYB84T%2BUiHHKdgb2GfMf5k4%2BgDUIWK7NOvM5q4GuRkfpmE10zaaWOekURY%2FBkc51IxGnRwN8hrd3bTn4JjMHAs07OZ9CHAKrJdyQAR&X-Amz-Signature=9aae1684f365dc570bbdbbdff40eda68c5bc2994e68c36b14e554f37484a21cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

