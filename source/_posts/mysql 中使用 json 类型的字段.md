---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHLNGGDP%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T060135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCLD5VODVh2TWu1hCW4NYUSoft6SGJ4P9qXf2fTLU75ogIgNyLeYTYS318zEuARaydqJPbWGjrv4XNjYwcNqjNvdCIqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCwiwGsOQgakXBCJFircAzhodBJsK82E6rISN2yZPcENJHaDfjxZ1e6URsKGx%2BHaGmIAdhTet0CuQznJO3kzXS7WmMBhl6UgORa91JL1R6pTEekPktz%2B47%2Fh6hQ7gpkpioEb9Zm%2BBt1hgaORWN9%2Bq7S4QNJBGSlGB%2FnzGVzWXZOl7dZjvbki0nhafRcd7WCH%2FbJfJKQ5i1BCjqMAMFNyoRzBde8PlRtvn4k7moKUbIcH7I8lV6H7PY2SMqz6gC4HfUt4am%2FbBcmfd1yUk2QTAdBnkZYIEJjk2b5gSWV58CWS5Di5j49TIxTALvdB0VWvlrXPJz14h9BZnqJV8Eo4cT9za2cxjWnuwVIr0wAd%2FEcoqgTeAaZHjkAJqkQU8zik2WRXxQxheCGil8Cbmn01WLyqVDUebtAuBv%2BCxo8gxR%2B7ozePRDuydYVrvJKiunH6B1ebmdU0Kqo3JqZ35nUi246zZdHtYI%2F6lmNDC2fG2QrwHUU%2Bv8HPF3XDXo6ksVvlLtkFK9FlGnlVmn1F9cA9DThBaoggQdeGid4tA1KjEtbRICbdE4pmj2fKS1uN3I%2FYU7RnsHSHlUBx8i%2Ff0AGNz914cp06OzXAe3qLJ61mfrtI%2BaHXHl4Wjcrdd0PdZPpoW7Ex6B%2FyrfLZ%2BNG2MIL878gGOqUBLudDmQNhUhZIENVkpasY0w4wLXK0BhYDYl%2Bj6PEd45qhtJyoLMIGfZMehETBCzc%2FDWCM%2FxSo2LTH1se8tXjtGISJ1%2BZd8tolcB80iz5hioPMVKQZP3ncBUFOJKpdLy5hE9RmtjF%2BYKGLT%2BKzEk%2Fu0IMHZ%2FDDHYYFmeroItU29WpbRXJJ1KDhxZpY9B%2BJoPmitBb2iP3osYNjgr%2FU33tTmmuOCP0A&X-Amz-Signature=58926aa0e3e9784f7cd297b4c53e6cc8425964bd64bfc16538e39cbbbf383463&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

