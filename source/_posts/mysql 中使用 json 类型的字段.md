---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XMXZC5WI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQCVTjAPSHLMxsnmnRESPmL3BjlbLPnr3gZicVmn6ZRVnQIhAP3K8K0eHb7GjKT7KjxuKdtxA%2BwgexOyOvm72pIKXVqGKv8DCCMQABoMNjM3NDIzMTgzODA1IgxgdobfKF4bPusJGwcq3AO7ONhKYHM6NHn%2FZskcijQ6hJLc0rWfPnRDSIYPzGwqXjwq%2FIT7JYAaiI1on31Fe0n4EU3wtnHaCI6MeIgP4WOs%2B2FwoR5xpxvLIweGK73h1EKmeclydniU8Lrvzvq1M%2B3ZOnLRPrHGvEcJyvRVMeV0EUAMpvXdI8BGoww2DvZU2qVAGh7Af66JsATOHFcilrzMwbF3RE6jugp6OOWWMX7BOaCJUoVDbXbFbT7ss5psx1Cxg8Fy3JSZTjICcFR6FXo1Ui2qTPpS95dC8m9lJTnB4oDKHI%2F6W6dWsfONZWkXuxdPP62Lonw9uLfdCuzGLlRzTsVA8Ge2FIkFHRmze12L1fqHexFNIvkFwILsA8Y3N%2BDjrRw3Hf8N9xBgrLKg8Kf8k5dA1ISunNfr7OWMnYD8oUgguKhJEONSXA9xLkq2%2BioKYG63cuo58aXFPVH6FkkgIMUjCzG34tsTCrjiScm2L1hXXJ7AldmmfLhJwEZ7htlYaPdZHc0NUC0UW4vzEsmslAp%2B3nPydZ2gfh3CXzrKUEAYXD5F79nBFHBncUoVeZunvfSfPKNYXShadlRtwmoQve5mx5e3hiOpvABSUoAY2Lm3cIg2F3E4e3FD48N9K3KpvrTPEF%2F%2FyUTpsDCl6eDHBjqkAbdnDUQ%2BpuR0Mmo0O%2FNB8Sf3Md7kaHBvfF6mi%2ByoiCrk1J2MBk5XE7lMiYb6VYscZI%2B2SH6P%2FIOojz5sp5fhaXGKWAnjigvC%2F1h8HJaWUiUSl22swgCgfCiQdhHD6bJEbgNmlalGO6JfBZGBsBfw9M6z5CfuIK9%2F7mHd3695SVgYyJncJaEhbH2Ofyy6MzQ%2BZqF6jj90DgTHEDVeLTED%2Bwqc7LB4&X-Amz-Signature=da15c7dc17e321da75043686e7689e213d5a2e9445aeea7a74f47905431abf48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

