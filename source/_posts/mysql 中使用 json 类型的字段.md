---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THKWRGYA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIHkhI6CSXOxc0e2coUNbwOvLIxNDTYvjyoY6CknLXqURAiADYlH9IZTDw3eHM%2FkPbVI4B0AFGmqKU6AFc6ECabN7KSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp%2FAwN5k5p53qeZgEKtwDsFr168epwgipKpQivQknWAsTdt0kJ99rI7DamLYiwInmwH5%2F3x1vsBqena4Nl0gb6km%2FX2jHXCQphLe8YH4bniFfO4sQbiHEW7dpuWFmDdFQZsHydDx%2BdfayNsT0w5QxRsMHL55OpgbVx385JotSBF548Y3Ciglz0qEayUNnP8ImBmAGXrKobdQbwst%2FnFMGR0yqxFI0ojBYIStiWYiNHCf9HEq5Pev07jeUHWez9nqqcbnToPAkupYK93VWwhfIaraKvXQ5xVUuMcTddP9fYBXRj87MFVHcogEWRm9pl4pkaPwAqVaIn8YFymTvs3r%2FF9AFtOC2IqcId%2F5xjsWHknCBW1YQ%2B5XpP1FNGNEO%2BWdthbTf6AJvyfDQdSS1Hp0EakaW1jklOn0I76C%2BgaewgYF1AH5rfNCHLunWdDRzuFpb7N2gMo4FqmWSf0ngcCgGLS0tvRhqCRGtatqvYIt26UQIxtEGbF38nYCL9LVZHPp4%2FvZ3rkA4N3SX8grnL%2FrrMAK7Ouoh2AacOJJtClX9U03pmUU2nfz8C0qctA54zPaQr9%2BL99ohSUr7Bm2Jewqyll1MCM3eU8b5IpSXncl9lrJFcBKkjquXtuMt1R%2Bai6LynruuS9s0lur8U%2F0widP2yAY6pgF1vSCvbcS1v1bLYDo0cDAVQXi%2Ff0p%2BDl2JaKoOWU2IEGxQ76vCCVI9IOWOPP28EFDQmnI34T2dkLbq%2FMP0XT3JRKSYrKzSpqjAEJKNHhV3DTr31xBqtTDtEsKTXDbN80XZ2i0tvSBsuAej2OEUCmgTetO1zqHjvCI7JVSM%2FgcpMUZ%2FMxxVEf316e3jOm1MmpuMFqD9HZL5lhD56tbuWncOliAPAaFW&X-Amz-Signature=bb4d97a6d23be61d91c0172cdb09b5ce39746097bc8a0820cfccd572838f8dcb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

