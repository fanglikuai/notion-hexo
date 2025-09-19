---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SV2TJGXB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCePKFNWJcCH7eTXLSYnkvLuLlJomyJtbVUEDCw7NX%2BhwIgdi8PdDsJBRsDVUrH3fBHsarkpZuptPZz3EYZ1F3AKlsqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBcqcEUAze6JLkdo%2ByrcA%2BqU4bJOcSfHfxosOLfevMmfN9Z542CliQoqFDtImCWwC8OTDHiLKnIvMY6yn808xpZJFCs7jD2LuiBAN4FDO1fyrDq6uDcyuZNBhWEgwdM0mqMh8wLFbeWyYxeKHUNhbIPFdz6fj99MoGfIlKBkkXimmzflK6eP1ka0n7iJfNIrSz2qImS0hHx%2B4W8YIK4gSpP124JncMZNmAnwcpLeyFR9FCJbCz11ZO2g4Ui2LLZMBvpsw6RGk5rhe9qG3KvCC6CapxjPQYAURbnLX7F9DsLHTliJHmrnQRSGRJDdnxcEi%2BEwjrIAaa7rAHTOjhbcS5bty0%2FTRtECIRWKF%2BWUw4yxnRJp%2F5MIXUGKAKr8iJZ4CzBQQEN4b%2FPQsIiugKWUUhdasTMjGqI7hkBaeLzWlrKVVrYU%2FMX7eC9xMFd8Dl8PpwGWvuIACdwM%2BL0cEoHPFoeIg0ss6YB5GNDUI%2BVVVxlVQbiGzHht4SGsX4WyyittnhE1%2FbHSW5xK7dGvZ0J8xUGiEHb0uI%2FkOznvmMqPpq%2B8bpQMoktam199IrhjVSNE32gt4yMiXJQ8MgqpBZDUMOCJNEHHb%2BrRibTINrkJyyurAFq%2FJUFj041bgoPqXrA8pzPFWyQ%2B7o2NyxYgMJDjtsYGOqUBiK8jtQOFWhIwm8yPK3KzyfoGI%2FdiAhCRZcoovF6vYvJloApEp5Sw181eDNt0WCDl5vw9a%2FqtUkEGIS9jckDVXWvvY%2BP%2FwHZRonOwV%2BVTnnIHhzWkAc%2F%2FHbfHkTMmAUfP397nqfAUTcaKRB4mffbVijQbpAdaH56eOUXZowUhFaC3lhIVDY2r2jbphSoA6qvdYpZ4Ab5ssU8obTwiAtY6mmFU3GZw&X-Amz-Signature=55e661ee52b2acb3e02449c67758d00ac14dc023b41aea7504cbdb271c7b9024&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

