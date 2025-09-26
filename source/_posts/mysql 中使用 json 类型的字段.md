---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPLMATDG%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJHMEUCIQD2gAE2K8lTYoS%2BaEDDqX02FNLhKF9KzsVzY%2FazhM8iNwIgcRjTJIu8eY%2B37SJVnfflGF7y2HrQBBG%2B4byNNK0xoBQqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9kZ3XyADciglqwRSrcA5Fv5Zvwt38ldaclNYixZWo4HoJP%2Bia4ZZDSX%2Bg%2BtBglAq1YvxUuZNhxIGRbvwAdMEizQzu2WbBO3hqCL0oUeQjFn1GGRKu1D3FQugpXlp5Cl04y6g2R6tVZ%2FGhlVsa5RdO28aeActN4M34acTA%2FQyRPkmx3iwKJuAh0Gih2b%2BnP5vmtvXc246bRxz0bKF09AHybGW3v3BO0G1LmrM4mrMY2yda%2FzsqFam%2FSchYH7fvQVyBYIAlB8KQKURT49UxrCMwKvsI7I7o9asFQ%2FWZ49XXEoGJm6SiPYJVDwfj50OUxk5DWEiGY%2Ba3xe1axNy6u6769tN9FhFS8RiqTWZ209StXMeMw639u47MIQaX%2FhlWYT4mhOmiaSelcLqQKhsf8c0QGDF2xSa8rwm%2FwZDN75Kil1LOi46fR%2FNpxNwHCjDCzPnazZdjsNuZ4Fxn5e0hNlM1Mn8EFDpPPWWCoePWhkmVdK4dFOVbKoodL3fdLEpsAxvQYU5A9Fw%2Fj8gAVmDNz0XB0PGB2jpoV7%2BG2ewXfAUXyzWkYKiPxr6Tz0NggFdiU6Y472PdseQofiLxBPapX72CJx9KvyQC1I6EBac8TzukImO8lPC9mInNgbI7x8%2B1oqaN9nHvUKhEW1N9hMN3n2sYGOqUBRe%2B93iysnO5aa4wgnvE9flcc4NxF3kwLi6CUgF1k030ga2ty41UEu4mPkN7NFx6UqMfv%2BLjfraLncVo%2F7%2Fw7yy4X10mn8CnymsVna7mQ0kCmWaLBmRCEL7aqYxYpdSzgTxFb1QxSq1z%2BmbJFJge7oi0dEiLoiJ%2B3mQu%2BK%2BZtQxVx%2FtW6X83gHA3cpQPVei%2B3Ww6wfRmGVZEsCWob%2B0SyRrvrBPlg&X-Amz-Signature=88e7755e02150e30e64fcb84cc4f5994f9a7244bf2326d3e607783d8fdf425d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

