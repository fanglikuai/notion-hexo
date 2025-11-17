---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QT3LWVBR%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7PYnf28qqrdAisqC30qQ8gdHxMx3m1vevZKT%2BDAXf6wIhAJfmhKoy3IWG0w02g3rIlZO1kk78Hryr6Lc3pp0P7e44KogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyMtZXREh99Toi0NHkq3APSjNOyf6AYtph%2BAE6t%2F%2F%2B2i3AH4zfuOqrLVbN%2BoYvvS8CKmIKZzVshUfADZhwXxV8b%2B0W2SIeAMlaQW9rnVMGbZ6%2Bid8SX81bYSYOSbEsnH6ce6x9h6RvYwZSn3W4DsC3B1JeZIqaqbZ%2B6fYuu5%2B6Imz%2BIQRmvXe9R4cPPfVobnlueakA8kXKC4MTz9IZLpWZDHe%2B%2Bgu%2F4GQMiUMIq6GsIOwb1bKW9q9NQUSBu3fEj1fxcDIkVLi%2FPHEQM4tl5kAXWJhyejI3w%2B7rc3wDbmQLkytPrPJ%2BTijPRK1urGqi5VTGP1K9uFFIdMk%2BKtbsK0Z2nFqBmfY8KkOn5GuQ7Y%2BHGX2ozlo2e%2Fy2ABMa7MgnCwZ3byTJ07UJy%2FbnrIzzUoFcLEv09KdV%2FmT12uTREOQUOkLwjKTF8AahYW8gi64rb4H0T0pAYKKGOMlq4PsshI4xxolrqU6YAWxgcM5DRh8eXMUWrmPIV3BDKp%2BgNpyRm3KH5bLpMWUPDYeLIACk5MihY0trJsB0BgpfhapcfHARUtfXWnUzMZwhJAtYWJ0lOPzXTNX7JcVLrjMMiDM81oFviJC3DbBjftJCKUSO55ewc8xZ8ZBcuFVc5gVqcpBKnRXgN9ysnnmhqGy5QZzCUwerIBjqkAf98KZkR4fLpJEL4P4M68PKnz9wT4qiDd7vdylQFspC4S8FT%2F5zX5JhN5YJ7y%2FV4CwFXZKida0ZCT3JDT4CCL01Fts%2BdFUgVM2RVKVmTIHUJEpQCXNBmF8Lrgk34HeZpII%2Fz8LmAQpkZQKmfjWKbwixYITo%2BftVEw7LBtOHnRI0QfcEZ%2FN4lvuCnZKOQ2Naw0OihmAF5I08nWTodJ8aYE0MgxHoO&X-Amz-Signature=f795a72dd37829183fdc7334f52f952925087ca1a7fcec1863e045a127addd8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

