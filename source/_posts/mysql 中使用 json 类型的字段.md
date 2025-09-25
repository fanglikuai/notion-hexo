---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHTLKU6Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHlcPGfNU8U59Ay1EEeZoSXKtbxH0w73dYMtgRoYCeavAiEAq%2BPvQgZRlNcGymfByb4sYIRNaUA%2FLzExdE3dPcJdFXgq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDBUd7AHrimbWabKEBCrcA33apEwHYO%2BPqcbkN5FbPoBvcVKktD5ZTKbk43AzJ%2BxtCVVlCNC4idJRzd3LxZQHXSv0CrLX%2FqvKHhSVjOjTRZcWj9bnnN63G87q8CGzQkznzaZrnJP4piGgA8SUp9VkxSij243YP5L7zN1z94k3fC90JQDZFe8C%2B%2FeCAyjrHUDETaksuXUrAghLIBwPDjd2ST%2F6%2BQzOjaEdOlE8mFs6IQrL5JrP9%2Fw8TLkQ3A%2Bd3x%2F%2FNHDfSuP1ue7T06sJ49hnFAD3UhUu0%2BKcbdzUXz%2Fc6ydL8tPKHK585Y0KYlgCKR%2BIqQ8PRw%2BkE%2B6v53okEISu5RvusjDpLswYhKdckW35Gg2NO7e0E8dwOkazngBuT2uLxitVGll3uhEJvzNcnFCZo7Tok5ruynJicDWz2JrTOj3ht%2B%2Bpb%2FOavoCCnLuUsuQF6lT5Scg3esyMquFzU%2FUSXdsk76O6S6IeQSEdQNf6IZbPzgyBWfezbgNzR690XAFDEVgUVhqMAwTtBg71xLV5fcSuL410qhhw52AONtJ0lFBdOIGdepRamJfb2lXmmsqLmCNNp0fxw2UY6L0aoPAV5b%2BXoeCR5XGXJJ51L949jmjQum8bJ3gLKv5SyAXDWx5NN%2BqFeZF4WLXSPz2DMJOI1sYGOqUBgd67G2fd%2FwE7SBHmN%2F%2FhP5GrIPeqFUGcK1IxU6vO7Icc4SgkUH3O1BtbLMbYqVHv%2BxF8dRYWyfMvpnGxTRMTz19WW9UFNlCb%2BIvt%2B0K2uHuthXUgUZutNGi6TspL%2FZkyuPY6O76Ok%2FY3sELmMfwiUDAG4IemRBE6CFKs9bXmYua9YiZikd8y5dG8KS4Ade2Z8Ic6qMj7cOtsL7%2B0dGGGF9rPY3mZ&X-Amz-Signature=68af616abe359a261ee7bf45984e8f1d35ddf8c096ea0553eecbfd7c77c40027&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

