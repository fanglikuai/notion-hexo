---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFY6JKO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIE6hYri548FmR%2B%2FHCoGBTfAs5cL6cC088pAP%2Bwhi7gTdAiEA%2FbVDU6rDcGRoD6%2Fvsf9p9DL9IDgHQfiTEiqUpZFM9gcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGKysoQBqDBIOHZKhyrcA6hSFsY9UHIod3hq4bCFJOnDZ9LhekF62Cgi958XvkGFXJkQMgbKLCgAOHi0SyKpoLi92CNqYPfzQRyfoBO1TDqhAIu%2Bse8k%2Bg68uy6K7%2BrQjEtdIM8JPobUiLKX90ZRq9r5wvpZFs6LxtR0q6rUnBcB6mYQqig9k8uwp%2BT9ABHYGmTKiFibjAjS8XO15DZjgZDbJ0rCP0Rvkf%2BrkWI2fVDrtVhU7uS8Yy8iwUqWwdKlBpKjBtGXEddH8x8r%2BhZJK7PuvkWJg1820cBnOSIeXXkFK%2BkX2cYR0OhRlP%2FSst62Z86oskPBCOxHvOqLYLbVL%2FB2jbCKnvSTQtf8h%2B9g3PcxHxTxDazwDs6MwpKMuz8iHXxAz1bp1VIJigWt%2B1nSrcu%2F7ubjDSlIwDkPU5ns9%2BRao0WhZQaK6U76WI%2FaXHKAKoPH0JzVpD98Ng4%2FDMvfE%2BrTrrno31vRoKcVEGMKALnyyOJAyPpiA%2Fs49oexaQYLMP8HCjL58hAMnj%2BRr7WyIaejRnRvLxrIf3HMubbUttd4wDsDJXuRO0%2BYszbEXBfZjqzRnw9l73ZvZSN3gVg4mjdSA9XroX37ZKRt6DETqUgYpx9wLXHgK3u4dS5Dbg74Fih%2FmHLaup%2FcxdmqMLbDm8cGOqUBoNbID7edYBPboKd%2FCLupdxUDZepY2q4SZ9R2Acm4SkqD5KhPoolnkE%2BFA7AGdspLWJPeDgkA5dloX9JYIfZy%2B7mL1VlqwygEkecS0M0B5UC0T138l55HMXqFPhW6DgG1WkW1DXT4P0mjuA%2FbEWFcAj1hsDNH4Ov9fE%2B%2Bpd9gZZ4H82u%2Be6WlhpcsMCfzx%2FhUMw6VAOH6TXRRDkxdv7G%2BUhmMQOWq&X-Amz-Signature=1baaae9882f3f2dd4ccf39a0d3bf8ec4e19d662830a9985566193431f5794419&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

