---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUWAPK6G%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW9IRl6ZdAFuXaBJRtrxYagJ4xpcbLR8GssiRwCc70pAiAOKCOMhZ5LByDmarB%2Bkho1XzEJJ8ew1BZE0BdOfk9c8Sr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM6RIV2dkX%2BMRDCE2dKtwDsQeIlnGczF4%2B%2FzbMKF2ZuL6O2HgkVgHm3aAgdgk%2FW0mVC2mE%2FBNzPatK8tKkFj2bxEBA01sb3bTnaTNEySeDnjTz4YprUCBAJ%2Fvxs0%2BsqbygbkbLWLOKVvy8uWZMsU6WwRCsz2I4nSlpDRuXU35rYsszFt2J0qJgj4t2qncDswuoQmBm6YpZWkxWaleKGwdDter66%2FeR0cpHYMfnqUAxJekMOq2lrHIxcnv133eBftl6b28J556cyEFgMx2bRMEWmX4GkgaisOuHK4aWOXayBc6NRf0JKOk3JdPFVj4uu1Vr0RRkOLFDxdApbOEu3Hz%2BicRIrvhRQrJvLjYv%2FyK0zals2WNfeazy1iCsAAdjv0Q%2FBQSgDZug8Jw1K099BPV7cEo7%2FjjIBXRR9XHH7i7awThoodKyCFZhWTJ7F48HwtewzevmwlmMerCBRGKZKxtRv3K3T%2Bhpi5yf6Vw9zJniEFyJKol0KFfqOUw%2Bz2GoMHX6sMpGrk9YAoch8%2BHfqDwHGVRAvT48Q5blRWBSqzhEX2Cm8AVzalh3lj7EUYYW3kkjSdAEK9GLm6mfIQIG6oG3Pd8R9pD3TYjcIXGy5bYqRt7x69Z%2FkGItLaN7ZvENnkwFj9e5Tf6YzhKeg4Qw1%2BaYyQY6pgGUMnYmwvjxAy4upsctGa9QRs6S7wpONh4BbiMChWgGFahMPy0BATG8D%2Fmf4%2BQ88BUnn5zu6UFHgKMNC3Hl9h6%2FuhxAG5FV5olrtzb2e6B62ENHFC6pLycWLso1w0i2F%2BfZA8dsgpVCoa8BjFQk1BMun2FUvlq76gCneTJXdw%2FMquy7W%2FfiULeR%2BhA1TW5SCPBt5wS5DYh82UEY7TmTFGtsiPWDZzF0&X-Amz-Signature=3e28151727092bfc0a34f982a778dcfe7b809a1e9d53351e8f49f5902c58e646&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

