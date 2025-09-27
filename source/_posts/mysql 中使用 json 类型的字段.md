---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZLLJQF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIQCQukkTyBoUYDCgFASMWelq186Bz1%2B5xYV7z8PsCXDD%2FwIgU2DWkKMHdsVSU1GVv1PxrD4BLaNRTrcatIGvgid1w0oqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLAIrdt50eXDBPcW%2BCrcA%2B5AYb79hPfIan3vyDfND9wB%2BOmb3ZLp4uzIgGc2YpL3XyPlINJS614C62CsCf%2FQBuArJgbOQHUfg%2FlHfimDGPHjUOdL0WXdTFnqwBXUGLYqoTmi2IYU9zwpUAh6%2Faqa75b61ZOU2sYtrYCPS2XVb6KDL8rNH2bjtOVSRanZj9eGJk2eQsUssXvG7ef2OU5Cr6EmDDztqtOklPwn9wbNzXb%2Fym%2FQidI5OwaVn7O9SFIt3yW%2BpCrRNKup8VgvYjcoiRqd4j3esXo03a%2FJDRsqK8L3IgQ32JmLpEM8%2F6Sb2T6PTzZ8gquMzo71dwhSipbzkmltAEgso1pqZ4H4s%2BVP3Nk9QYPCZAgwdde5m3i3GMwk38bj3rWAvT7cF2Vcac1q9CbzrTJPytT11FY%2FCNy4HrfJ5EE9zXO4bg7vvjOAsnTRT3D3UMCAtDNmlsY42NcjZ67TkIYxWttXc9i18l1GcnSt4KSi7jrUgCPupztX4i%2BAxRtbbLaLc5FPUmIa2gesn35UCz8ptNO3ylXE6g0HTegGTcGuA6oc4NfSFT7Qs1Xfrq1FItSXsnt4lphzVzdnyw2Bsb8qrUr1upC%2FLPAlWlxYHMmjwODsH1RO%2ByU9RLJJ1V%2FpxM%2FYsRoWcce3MPyp4cYGOqUByaUnIOXK6xLfWJq4qNrjlmxF92KER395RHzuWw%2Fw4ucqdZSKsE0Y2nWX%2Fi7y5aj7t2PQOQYkzu9t%2BFFFfkq4yoFk1zkXrtwcguTd44xX7mjD%2Baq%2BkVbhpTsJdkqlRh6K%2BoHc%2FYKhY2Qorwnd7pA6cZQwFMCUdxHmoUiXOOi9BzFpuvNLxynun1ezCALgwLFwa6DoBuAepnPaF8WVsNc4LcqQ2JI7&X-Amz-Signature=0ec44e95f06d0f8c5bcbb73a30edd140ca3c99a127bf8d812ea652df579f9def&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

