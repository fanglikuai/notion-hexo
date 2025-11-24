---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654H6Q6GE%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFO8MtAngPlfiyGRsKEkMoh1snkVBrK1J95LP0l3DN06AiBltyqVfLrK80kdabmq5gJJ9iVDw8qj80Ca293P4JUurCr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMniX1S4MXzxLai72HKtwDfho79FP07Kf%2BZrlNAJjYz35hBQvOJoT2%2FjShQq7DQ6%2F5U4yn1KI8Hh6EWVlL3DwSvahAuNGPu4%2BW0dAJWGmofQ0fU8vtxQTtvECsu%2BiQAXfUFsK6B5Iy3fBnEUBeDh%2FMYgt2P98nP%2BqHffPyjCwi%2FL0e%2B4iaUddum4yzU3pAA4AL3IetaNoOZ4LOAnDhJEwW%2FP8zyw8uyxG%2B7H7XQ2nL2mThr8QPNT9lSUw4QtiTqPV7yOU7c7VaE3daVsLGoCdr57xzXzLBCxlYdLkj0czUmp1EldSNII4KyogVqVebbaOQ9NpQOtAUR%2FEmDZFaOFIJNwWHly4DhXCM98zEby0LupQZ%2F5DIG4FXJi%2BkTlu36qsk0RCEzRtElG6ML7GiQ4JJgGNrE%2FBLbiCPbcMTcnCiecY2ebX3jTqZrPOYXtEklH0yX5mzHPgtFjh%2BnUjzg6t2rFL0TDrIvvx8kBjr46CLtu%2FXclpdf%2BOQ24kg9VxW%2B7D5EC1iQUu5oABrhgOgpYg1FvoN3iLxsUKetrjrYgA4Pb3twy5Zi3bDiBFy7l5z%2BSSBw6LwnZN%2BET07tBXlqPDPc%2BbWoAj56y2PyvNIy8qwBxyUkrW5q%2BOp4a5cQy6B0AGwUA66%2FicKA00juRow7quPyQY6pgGyJaBt6F8PleaoBBfJ%2FpfCm9%2B8kvmiwEbiYq%2BksTD3d4jmEx%2F9FhTDByXKKfgoORDJE1xuYeNccMsZGMLQD4MvHMfa9wC%2FievmQlMnCQfHdEhKYTWYKBcw2Y71yNqmsoHinj19porfaJ%2BbkDMqx10xk8Dg5zxJvAIrC0I%2BIdn9U3kR%2BhRw886YCXulIvoNK33synd6CH6nGGqiejMxvw4Z5FPDwlH7&X-Amz-Signature=7c730fdee51a33059f65578c6ad5cd4717b00751eae09e0a13e2e7ae88ab5ba0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

