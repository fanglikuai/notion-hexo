---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCXQHK6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpO4EJcgDOOtPIu81raIBT3lRwRuDvd5MLieP%2FS%2FexngIgEZKBAv2XYS5uelzI09w1GQ%2F%2BIJI6t%2FAEeZBK%2FIt9n9cq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDFNmb%2FnYs%2Bpn4d198CrcA7IQZdO2cVdBlSpMjrzKjwrhQHorohfyef02zxsHLV2j4LWILRRZO8i3%2F72uQ731DZ7d%2BHsjuz80ndNc7gBy11v8TJLKpqeRJAQskwiT1LygnxCs2ugWThLzbOgoVMYTUSOlteNSxMUajn2BQbKpKd9QBXd%2Bz53yEvCVtZMuUfJAMcRxYrEl%2F8MwI2EmO9SmonUzv7OwvzIePSFMjOOEYMqgGBvJOILCLXwFSKLeUj%2F%2B%2FWmPayq1K8ErDi%2BTfe9CO3UvKcRZtzNKvMPtjP0xw4Bo6oMDsrDOtuNc9bwtrBv9I5GZlpWGMwv%2BAQzlL8aOfJ18tk0egGm0QHLk%2BEXmcWEXn9iRtK6OGaM8oAIE6wpKiBsHSx56aZmVs6vKIWsToaO78QHaJttx6mKL9w5079oDAYmDzXxsJPkAukf9v9I8nGLgWiEmB8kbJRCmmftEt9eWF5haGKzi9upLczrfEwlg1%2BkNLjViyb3Syjjgil7op1DYDG58YRIxtOa8cLtyqvbAT%2Bpyjo82w4Bb%2BbVTMhHcuOqrH8YR3bb31o2ZNTJs46Dr4uvrrdlc7KA%2FPysvRfvDT%2Bek5Q2P8jYtMRfuufEE1SIZid8nkswltxAsclEPWQGGFxYmKS%2Bbzp%2F8MI7u%2BMYGOqUB7ecPulxFyZtYwy%2BHPlTZ7YzcR%2FOEHYT8MfvkeeN7gpKSiexmx7xu0xVJkJAUwmYR%2FJNMhI7hxY1GZ5BnG7uR5g%2FACZoHSmec9QNJ53Z3KFlQ6QLfg8luhFTC6IYWG%2BxHgVLhvQzKatgz1tESgYvtbdhITM7qDY0fec2lvbazS0cQB1G7%2F1HyuQ6cmA6fdXB3LKFMzZMaDfWzIOMQiFAL3PLyhkC%2F&X-Amz-Signature=4bd36ccaef2c315ca923d881194e02edfe797734bc60c503e1f82c3a5788d794&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

