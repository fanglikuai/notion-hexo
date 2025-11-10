---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFSMYXL6%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIQC3CV3BU7bVGTuDi6RwAoGQfWnLdALt7N72m5kEv3hJgQIgVO7wWB6wKInPmDu2YXjtrgff%2BKp4cZoUZTk6NSQ9tiwq%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDGrowzDryHNnsKHeVSrcA%2Bajd%2Fw5tS3P4g%2FiMiBS5iKtC9nDi6%2FgsfJaIPg9joYXJW3xthIb%2FRFY%2B932Z58MhWUm29mkcLwObriAMpuxLMTJrZFkhxvX2WAbWQ%2BqeQtmeBF9s%2BWwOTthRLDFGPUbHw7Ot%2BjRxJsWSUoOF%2FXq9UDtVuVP9ljJFWqtdryy3pdjllYnZrPsi1pl9bZ%2BtqHMFTHmP1jhcGeKDg4R29X0yrjd8A3ce7cLABIbUZEdrsI9ObnUyz7oAbTsjIECDK5JOr5Vw7FxR1gjniyh1yfypw6ogsIqOISN7ihUdDa9VzWqoGZZn90yy4nPsPKrV4U0k5CSDke%2Bv1I8ub4gmjwhlR%2FYPdP3EKbG1UwkuDhC46IibS1dEg0t70a0rHqH7skTMSff91a%2B0I0fU4pQW5zRXbE%2FykR94tjg3QsEdaIRQq9Uw75S53MPey%2FxhEdepu8F30qEPtyk24TbrWvFb8zdnb6ALuAnzUhQp1liGtwmFUStexn1OX5Q6L1NJCO5OtNnmlW2T2SjSixWyIGNxnDgxcr4kKl%2Fu004bI%2BncBA6RBvIWzJ2UTE6wa3t4IYAlteTVb5Upne8dAyYEJdURVFdX5x2ZNP5NJgsSdrfT5mHq6Yw9sjJjKXtXW0PhIC6MI2Mx8gGOqUBRzbzC0iEmMOHAcu6w8Il%2FLmniA8jNkW%2FI4jX54oc9fR4Ujfiy57o3r9u%2F2Z41ufmyA1B9PeK71XON1fHpmgmkQFuJJ%2Fxq5dFi0ywh3hFX4GVskCyerVMR7HVuIaNAk8gN0zv0ctDFvOyAa3VVQIgBAGgL8kb%2FtRK1T4cDbGoOHkUcyZ0dkkuNOyB2TExf%2BqLH2pqWVisXlUGRdlp4R%2FE%2Bor1KsXF&X-Amz-Signature=9de5201d65a268bc1dd1c0ad712be930829ebe687d850f5873619e5a1d16d390&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

