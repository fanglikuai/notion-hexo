---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCHRUXDO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtVT0uRz8wyAZ7hwnFS1ig27Y11BKMWPskEBWgu53C5gIhAK7EEoxwACM3LFXx4PUzb6gbLjRhkt89jwavJTJVrP8uKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1%2FH1hc6LoGqqaFsUq3APIzP7k9SGmbwwJy6O5rPUfZn1lU8T%2FExZmmVg%2Ft4Ni5W3AVjixKjAJZXeqR0839xBmqBEBto3VjioRnV%2B13b4wxZrHFcPLFXyA12EK6%2Bf5U9rd6a0S0Ii%2F8lMdYYib6%2FOXAdXWvU3xMbFk7yILEML8psa8fnrDb%2B%2FRgpFLdJLIiUHRbtgH95DtHhVZpQnkAWGd9C4ysV%2FCM1m7poT9lVfZsyM8R6OSEJq%2BcStgBAgl5zJIaqUtv5l4LGHejg3gMi0o9xLo4q5bjGKY8Bbwp8AHQvoagprWSATGqoy5%2BViadrJIhqZTclurnyn4ll3ZDJ%2BXRnq8K4VqunPWBd6ORmIImSR7LZsigdQtMjhI6hbj8zLuVKcfW4gQw9M8ycZv0tzqUm%2FWAvrExhhQN%2F6Eoa98NwzE%2BUSwX4PcDrLDRRP%2BaTgZHnBxsUkUywOtmP%2BYpIWtjjL96NTIL1tBNzTn8%2FgxBKuVkggtbDkvIvnmdaOsJbEPJ38jdaukKpk1lqwki9oDpl5iH96ZNInpfudvAzPxaUhVoa28d8gwazwPamdAH7%2BnLnUZu6qx%2BOzUd2dvqf%2B%2FZWjtbfuxUHWtCvYi9kwlXoVkjdWRqXuEhOq%2FCkK%2Ff3elyRwgMmVWFC%2B7ezCswaLJBjqkAa5rhqhppyK3vsJD4%2FO1DAgOd%2B6d01Dbe7OjUcCpiK818InCzDFNb1kT1oTHe0m%2F6kMXwjeI0BdLmTBZ1VqvA1g0dgYSTmpR0MMFAREv8bY%2F0AegztzHx53se4fGZPOEnAL5Emhiqq37gkt5Y0DnahGQHKvEkCeJk%2ByyJTnTn6zehKCZBvBgT0FbvtcdvuPfn8bWOj6YHqkhN4ecv6NqwtkkBSeg&X-Amz-Signature=92e981d67aca2e82add11f23c0082e0d5b44b4e8120d7e362d0e5402d5cc5e01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

