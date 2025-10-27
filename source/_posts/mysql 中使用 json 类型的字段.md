---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UESUJ42O%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKZide%2FnRARIJz4MVmD5AKyyMdahIgIcE4GIzOg%2FiJrwIgLDGHdn0uXuu1gKQh4HBkvHvxQhfz2iRB8hRTdd9mXvkqiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOsy3hXQEzJY6F7myrcA8hk1rgJh%2FYX7nqD8aSa%2Fu4rrzOIPZxkjF%2BEHhH9P3zjBhv7%2B0eCwI4F56T%2BkhWX%2FGb9tyGpfXuioPOGPA4MOhXgNMeSfbYtYv2TJn724EmNOGP72ZMQoQqkv0UzhoE7tb%2B%2F1aug30cgLY5RxWOW%2FwxvAcNg3nO4VrraQB9DH2fvX%2FIp76LrVQmI7nOtZCV%2F7TIhlqXpdHpG3AbvhfYhrBZEmmwrs1KMRnYnxvJYyzpNVzvsVMhsOHHzd5XkXl2tGNRo9MrXz8WMc8%2BFTFO3Xg0ymi6E5ehahoyzXSDRBvBfzQbFN9x%2FfNe7FZS89RbI3JbQJ4254fXY3cKXspWtiVdTVA4BsKEH5XsoJgY%2BpkUkX%2Fzb62Zy6bUc3FysFWzogpommoruuDUyP7BKHcwq4NsVXoVyKBaTFhgfNrFlbwebYv9CZexmxa1KZ6kyGE8y9OLf9mhGqaCERlzlSK6kD7f2fHiIoMFoxppOM0Y%2B%2BjOva%2BsANdc2iOkQhk1ECK25e7Al0KVxUC%2FAJxq6sKLNx%2BRVotc23OE%2FFqcOTAs7UPyZdLaEnRTX5w5Uzl7UH84MvNKFg6ruMCNFr4%2B0rHqFI2NoqhS6VJoW%2Fs41TnRo7F14165%2FvlKn2g0zq8O2MLGi%2FscGOqUBlsl8zKPzFZcuDwJy1Po82CyDFFi0Qu8fN2pzVCBIdJ87Gluvk32XYZeEAnCT%2Fnfd%2FSV%2F2VxONMn3JKcj9V2rQqkrT9H0%2FjmS0Oi5BC3ffj6gnTf1SDDVn%2F9p3sGujjDSjlgPSHixyBlPjUlLWKjJwqDBr0Fk1%2FDMr4j7E%2BNRx0hr9rbLV6%2B0xnkYatwnEFk4p1Lzxfsn4S%2B9klXjv1%2B6HVF7a6yK&X-Amz-Signature=43152cbfe2b5630d5717b0100202d87aac7f3e21539afe2eaf867e2fc9183044&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

