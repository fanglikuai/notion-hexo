---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y3T6PYU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCIB9CgZh3H0r83p%2BOMC%2Fd1Cu9xoU2ALwEl6psc4eW%2Ft82AiBRhZmW4QD2OvJin81ht2rpt4DHANbVpC8SucKw9BBtvir%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMx4PLUWAw5lMh4OeLKtwDCK6%2Bx3ZlUykz1BXY6rreNE9%2FOTsebeHJwkoZVzY2OUyHXO84uOVabwI0SNeMN%2Fv3HD1KAvcDMFa2dv4cu4xb21aiFI9PPHtJbhzvZSXiovIx%2Bdx7CoCP3zldSm35zPTMhry2Zuhs6JBVMu8BnsOYTd536yuKSGtbqNd6Yk8XwkL3cuSOuS9LhocRU28sKqS1NBWggRarmCMZslFzOEWhQJUmNpY1l2rfCOL7tEMKdJfOiXOIbxSs7oYCtgeu%2FD31Kz61ZfxQKSvV1TeIUMw7p23gxMiUXtg%2FE7AfqTfVNZtvwrlcQv0VFQ7%2F%2Buxrhw4Q7LC0ZIXxEKY1Hh2JUVTloSy3rXEkP%2FPZNpHes24kAc7Ps68we1zGH99dVbWU%2BiYcRupK2pmuXEmr%2Fhi5BZIMwze%2FwB10xiU2zn4WVSnfMx98leibcNG4AUMPJlcAs8rnqMEQvetqQ8nC9Xk8eNnxQg%2Bb9%2Fji8TRuwgYANRcBxOr2bbzBo21liWrxFMDuYnFRvUJidgKtO2Ygr1l8rDRJR4HWVyWPMqGAdi6yzKVyg9erZyskn9qU2gMimSWwhnLU6CNQUpj%2BidQoZSE60zK5b8RBifCMx%2BzMOFiwAVbBZrugH6Q1G9Peirwjwa0wldDQyAY6pgH44bPOCIVw9lbwYuLGX8hu9WFjTYelcRwL%2BUyLzmCu5%2FegSjW06qAvVqzjBXPtR8w%2BSQxmPZCCThrhZXMBwIPifq8PTCqgq6347wc4Z9tbwdSRRNgvgrZ6Wp1l0R6KaWV%2BAmzKDOCUOYZIEza%2BHcVUeu7cFNQvPMl6V4Km8PLdes1mkbEuaOEUhl3qQyZojRyJGTo4OmxjRl%2FPvJdkzLmU2b%2Bfkqwz&X-Amz-Signature=e7acb8db04985477dfb760dea7f05d41732941f689ddd29ff0f2199285b44f5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

