---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY77H7F6%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIGYBk1Ah3rTHBJuclVYUxXdXbMVeFZaHr4ezWXA7%2BHeJAiAqU3VXRV4FlNjjlE010QG7%2F2uU9bN9ipn8DUi7D2DZgSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMHfQ2EF45bpwcWW%2FpKtwDgcL0x7SCcoqRTEuowV3blQzhGncP%2B3s8lrv2umZk8PgyJYfrklNakkl5W06dZTumP6cf1aWnz7gGPdvjpIvw59U%2B%2B4hCc3r%2BS%2FU3kH5%2FZplAXmNj1O1wHd2nQfffkvxaXsA1fXPdSx0t8ObCpoX2kv%2FDXQIJb8aFmIbCsC6OIFoOsYwHNV5UIQ2gTfSw961LlwKvx%2FT11wKAAzkl2xh4apmClEeHq6%2FNRNHR6Q8DyCzxPIYZ4aacIjpyzRbL%2Ba2dF%2B6oAXJRefgrBob2Bh0WpnoXfH07Lbnj7rBCmHd9SRv7ECKHm1MhNWn4Sfy1czptAF%2BOreyNMZ19zQL7ZZ74TyIxcUSTev12M80fvy04pg8B4IUwrSBLj6yJs2DfXVbCF5MH6Kvu%2FZi12tf9aIqEzxJDv%2FNLmF3G0rUDpWe3daqTjs%2BXmgRmPj9vUDtD%2FZ3qmrw35s3k2HaTPfL0LfBOExaWO9%2B9fG2%2BhRbPxW8FOzYkCxoVMcysAbCk%2FrXjbMioQB3f7ytMhWYR8fj4M5YSuio7gTg8CyrM6BMem7ByDAmKlMgeVrdvYJfihOut7Fbbzx5umdPWMy1GFjffyIML7jGpDBPaU4YWnveTFMkCB9h0wqNYbL6xSS3B6q4wtOqVyAY6pgHV%2F1%2Fq4HGEeNth3M%2BoDImDpVEuc23GNqJV009yXBs8HVNQZjmlxAp2Ukb0meSA1F%2FbtwMhNo85YYJc6f9xh6%2BWuRhA%2BNsG19zKsCc1hJJc5x9fWiSexLM4vBxPtQCdqpyVIqQrk5H4fCURm3UOsxw%2B%2BrjdfddNyM%2FVbke0GYbqrYJNHSPM2CVhU81ff30WgPA3bry5yimptnGHRMG4TcKRx2cJPL9d&X-Amz-Signature=b105fd2918a68ca23ec62c3c1c27dd6ec41e89c08df1979c67ad00669a65686f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

