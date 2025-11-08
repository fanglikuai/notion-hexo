---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KZIM24W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T140056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQC2cVIuLw%2FQVsp7JMG6OGB13H3BKgK3hJD%2FFopL5K2EogIhAJoCyfs8AE72cEyW%2B1YdouBptzY9q1UAIIMOqEzlh1JtKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxfPXI69cZBDPSabpEq3AOWGM2iEUzL36rSD9jqpvk7RG94y9uFdvsLxMsCI3ot8g4psLBaQ%2BUrFBeAxpj%2FmXgSXkK9nNWX9EQU9sIjsqh8q9%2FU5MMz0JGk8vovFGWCIiVCqN5v1dGbxsTY7pO7ChJC04wkVif6uwMshGyWavcREUNmEsowsAFBk5plildcj%2B2v1AKRsnSIWfleF0MAtrn8SBq3aMVRE0TDspA2uPJJvITQ4dBcUQxOqz87BDGW3gsq3cVtZvF7Oekvwmageur1ak5BEPAhh8s6rlypZKiMLumOvocL%2B4BnK2gV3Dml0V6rHyh6AWhiLGIqkfBYbzc%2FbUf69%2BdaSDZN0tI89Sv%2FL0mBzZDoubNmi53qgKZ62iX9nGpl%2BGxD6ijPlSCVTlA%2B0sdEWOeYi4s%2FuKdfPOSlrXJMdqrLnExPA0nJMx4xX1rxEMyck%2FxLM3DMQoZenMxC2tDUT0Rd3fxaou%2BrPvzitkfswMF3CNrir8IXHeHDgrkeWPBXNKUwyYonoSXca92TnI%2BhPp%2Fn1ghntusyqVvRgHB8o34NWs86XcAj8aOUwCAyxOFC%2Bau01GbADrwx1IaJ9%2BvCbAqck9MeNKAYrsmJ6%2BoXvEJnmW5ntzNg8R%2B1K%2B1cMTdAbTowPYWePTC0jbzIBjqkAfV5MBcBo81yMYNtbB849J8ctYBF0ojeGqcXv4grzuVGW0A%2BgqWY5w9PvsmDK5YsLhfSMVuEtMq9ZP3tOP48N4Bl6TS4IRIniGx9QOHc0seeszs%2B4UnTbE3vRApr2iFJXu8fk9iRLLrxY2e%2BNmC9sUJH%2BbBpX14sy9Sv%2BHJCaKG%2Fuod1GIjyyp3aJuKAsnr5J5CyqycDa1%2BtDTex4FTRDzlwV6GJ&X-Amz-Signature=ce699c2aafe8933196e0a69745d0adeba6ba1b926ce6c78beaa640856cde98a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

