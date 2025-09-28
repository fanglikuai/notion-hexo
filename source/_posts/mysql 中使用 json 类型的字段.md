---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWKP4NSE%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIFApd%2FtWi4yUwlw5MOgffKAaK7LnPToSFRowfrDc%2B7ZYAiEAztASdUL8h1Rp%2BUFLqHqdDZRwnYqa8R7wkgPHM07t%2FI8qiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKz1YfXsBjYSn%2BC29SrcAy%2FuosT%2FWavINBmF54LUACJFXHBQlyRcAtXaCA9hOsXrDOwnZORRLAKMsz2RfgzYrKsxkfxXajQDDtK5SO2w80bq7LBlS7GT5aWV2T83twhvy6gAhIAT1YYFLBo2TWmrzQPBpplDPSPYZLvyzBj0JYJG%2BPIU9ZOY6GICefmNT6YhIAbgmJGDsE3SlXKmadQK9GEViB%2B8wjb5Cwq5y9mulcH7zLRBF77oUSaqet4lONf4BSMCyql3%2BYDslb7rJs%2FXplZbcwiAFDEyGIuxMi9jchJG1a%2B7mWJgZpKdIgmVyYlRrBUiUmTcNfBSWuQkjOKsIIFN4I65ArYTyv3fi5%2Bh4i4yPld%2FvBtIE5C3s5oL6wQds%2BIs33RF4QhiDUcoha26MAhf%2BqPt7voB6tz8R4I0m9DkY2tOIgYastHlgj%2BC6bMnB2Xk%2FmB%2Bdv7DblQldoFH0iLaw55NmsnADWPn9X5vwAK55xOMhCki5eHntIgKGegP4XbY2pL7DE0jIEc8lMgd6OQ0IPWrDi%2BelW5KrAKm%2F5TpvOqSGF4uUGHsuTmeUEAVpq%2BT6%2BAvf6lt7Xk40eEilzgg9I%2FtaW1oMZJ8ziwXYbCrVGpWL3msRNLcHOsLkjNpZ6%2FkeS8O8TI1cVrGMOrv5MYGOqUB7tvXISg8St%2FcPjstKJokRsJGDe5OTfYLjUOP1LyIjCh4KVSRTkmczbL%2FchsS32MhtkUN%2BNpDaC03Wn7z%2FYNTwKYKLeVwq1Vyn0BnPKQ3wKvmTKfI7ky7QZUxrcVdTmip93qgIHNpdmPVmbmJZ4R970mkLw5jMFFI0KAE92UziTkyEaD9d2Kx8MQ5dS4gePd2pJOnB3yjlNQk4GWl3rsh3i2Wy3QU&X-Amz-Signature=b5ff9d48448393649cf41e5a1b06f8c44f904404eb22fbbf90343588b8542bb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

