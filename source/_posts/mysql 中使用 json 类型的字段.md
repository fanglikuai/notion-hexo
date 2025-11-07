---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT5WQ7PC%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGcwtBm4m2rzNxRTXmsa5a73bf5yICm8%2FeXJlCohyAe4AiEA8ewWGgSoUQ0Zcz21J6vurjg%2B63vc8%2FMG0cHOGqc0byEqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHxH3oBCpRDaDgCypCrcA6gHEESsMZhwh4b880uCF%2BIVAP2rAD1u%2F3kcUb%2FmRE3NP6LuY2RQaH8nExhAN%2Bo1iRUZcUFRF02SWpXDTZ%2FaC0gd5F16ruwQ5toNoCRwMWBARo9rYDood7l8LSUsRCwOFnO6Y4bDQ%2BdrvefWvT3YwdLZVP9M3cB27UBOy%2B6gzsfDNf57RUJ2ODya9p20jw4JRPfOPc2pn%2Fer%2BfVz6C5gcy%2FA976f9CyZIKUuvhhUtj3HhLe4a8ztt%2BsOciHGFcE8QPIgLIgj1xwUm%2FaiqlPZzovIAO7%2FOkLP0LXO9BPmNySS8EB4SlbS0JpcXzW3uTmlIUdgPexgPavFX4voXwQaAhLU62haiXbrgB5xiPVu3ReElN6OnD61A%2FQEaIPDMWfafhkEKLHq%2BWVWzGetbFBwwH8WbNZl73%2BsGsBtJY5h7bNZYJcO1Q6%2FIdJMuOtM7RccrKMFEuEOjRghvS30bIwvcBpDlKjAbSWXAUj7orja1fl7RM%2Bvaa7plmzcRZGakXi32fADMZ5WOHe837WHpIlOD78siPZNGZ%2Fc7hch3hXA1OfGSVUXWRtjqIDFBo0GBKeRet6Rwq3XBnE%2FygXKqPEtOkovxdm2H5cgiccNxFbzSeIa5yljRJ9%2BevLbZSbGMPmQt8gGOqUBuqJ%2BdNhvy0QDrkS2l0jucC626XA0Y6qEZSIrSUWdTM677HCL6nGTT4pLLNC6pqvMTyxuai3CPebioW0jMGzFsZ8RrMNKrJwYJ16GgbOViECbIa%2BTGxOXtoU8375tHlF1iZMIhW%2FGzm2epz7494I9m%2F%2FEU9LjF6YLkAOI3UbT8lnqwKRtnx0iFzax%2FYmsjW6y40tvTbX2TAKo%2F8A39Xs7nKFaN4pO&X-Amz-Signature=bab6b622c6901dd9a61e30409a5d728bf08060d0d28da833ac4f5c27df3a4011&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

