---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674ZZ6J2W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICjfWpfZ45bNaqu%2FMVQwT%2Bb3GH%2BsRzVsaEW94qRVtI5mAiAs3Gc7L5EwxEbR0%2F9N%2BWsf5GKPujHwyVJegIhiqcUo6SqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOsIG0A%2F%2FxO0l8L5zKtwDrZ0AwYLQViaNlIPs2%2Fs9KOwzn8y1f2Xm9cZ5JWuB7KKR1XowcE8iJVS0uBsDiYzzq%2Btx3bPq2dMKbXW%2FR3Ch4HtrGlhij7SKOp%2F4BzcYrmdgAYAxSummWmDT77tkGtx%2BpMnsMy6H9H3ATkjmtah3h%2FF61RhvGr%2FBQEX6YMDiKsmG9cZPz0o7NDZLYG7W2wq%2F%2B9Rgd5nxroDlofPVCxn7KWTi%2B3zNjoLJykEm2ElobIyCRtKo0Ezt2gpTPXPR8L1C8R50%2BOKpK0hX0pSuE4ZOORzjugNniBgIFpKlikZaKVRRhCxRe65CG4buwz7w%2BoEpIdS9Z%2FO0IXPbZ1HHInphdrZeNta%2F0MIA7ZY45rC11fn5%2B3YvqphY04Pg%2Fkygk5sCtpeqtat8KOb1uKWPJ2Oy5uDtNF4zlU3d%2B0V2jVHTGVBWJWvrVFLhMXGhhynxumTdS0%2BcsXLmB6ejcN%2B6WHegCA8Sl2%2BcCnyv0CI1FEHn8by2DXFkS8MSyl2PrULXUFDe%2B%2B4%2F6greg%2FzTjlY4tEs0g8Wzp1848KnmT91EdZ9XdjXMZ1%2Fo1Fypxr1U6eupX5vvJVzhRR4zudVr02BeSB4daeE9H5vD8hjPxAlVb1bbj7G4h9PyISZraB%2BiHUowhYD3xwY6pgHzLeuUvb1eate741xfd0KNMd7vvvvdDU%2FsGd%2BVjTijurh0jesG6Ba9rqbHT80F18PB0yg8a1RMGm4vgtzk0xte0ezMq9rT1LxplMf2pZ3wsMePb7Ga8%2B6QIh1TNa51nMz9oL3xWt8pUoXlR0kGuFovjUiaX3Sxf%2BGw7vvTNnEjsMKHqgRqC%2Fm9ZHuy3hShi%2B8pK8DlAUvNqQnZNQsILb9spFL%2F9ozY&X-Amz-Signature=109e5d396a37f450a34b0af6a3a02537ff6d2c68ac5d37f705515c0b689211b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

