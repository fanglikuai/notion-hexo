---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HFJLQGL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDXwRfNHMXEwvnG9%2BuaPheaqL7%2FIOwUcY15V8A3T0KWxAIhAJxTslicFmizZF1IK4ZtAVghhT2Ao3e9SG%2B9UNgyEdmdKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzy77JCxrLDSgBxcmgq3ANM8T%2FLwskN7%2BHQVGpSF9fQXlGDRB1bxJqjt3FXrJq7Bfjx6Ts9juE1Z9i0xwCU%2BaphPGf%2BIqXOkkJpod%2Bqh%2BiwwX6SxTpvo5hl0De5F8dTr%2BD%2FmG%2F2N4vtHWx%2FxQObydaeDGEOE2PdAfalJ%2B7%2F4enVr0wciq%2FMpYcQ8iRWVfCHmgKCrp32atfkRrKyPnn0eefdSZqXXi8mrqnhqlyW%2BOZnUNYi5f0eB7sQNcAWEHBT8UuXtnLovGI64sy2U8JEwV5%2FgefcAwurs%2FWQl3N6zZbTQrJHQCd0sTIYV8RhtmjRRXT9L0EhNuXwfZE4v9%2F%2FWQVCxjyobvUMPFvV9uOvExXnxD3xZE9c1d6MrvkKMwFPiLv%2F0CMmpC0wo7pMMtVoqGjcuFmriYWBPpEoGO%2F0eswhfXAGPhfbDqYhoAeKUmFtAa7dAQOPnfR0Udw2xNuU%2Fz2AmfwUn6uoy681H6nCc41XCOrr%2Bj1NwMn8HoZ3hkyc3fU0l4sK50Coj4dxpbe8MHO391xC2gP5FJVcpKORkdoG2a2GBudANfeKJW%2FSkVSzzEGFJmMssGn1mcdIQfgOLVyNJ4gpsQgotRLLzCFXAxB%2FNl0jhQHnBFsk7PRyFemr7qJkQmOUbtOzTk9zlTDWj47HBjqkASwhdIh4rw7IkQZ%2FFHPiZ1Z3MwZfuN7Zbnse8Icurx9vcMuZnQ5JOe4CJsfyDoe2mIVjiYjYfDLYmWwPcUtTokeNdrLxo%2Bq5c8yygBMrH3KooUxIkuESnIABALui7bJ3s44f1dCKyy8w2QP8ynbTTJBf1IJRswcnwnJrEVgZmDbz6A%2F4X2LagapOH7s0elrAXkuUfMfrUng27aWLAp8mGdSESXeK&X-Amz-Signature=589eb7c1bbdea68517eb59685460e43055fc42c3e6825a372ae4e024aa9c3c8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

