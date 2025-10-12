---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HTN2VKR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9nJho%2Fz2OjA4EkYe7N91XqGnPsYUyFupjuVduAfB8ugIgAw6rEq2pmvdI%2FT3Z7LqV0RjeqyjUCTaM%2Ftr80h3PsLgq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDKBdP1A4XUFa8QvJRircA8yVlHAYn9NDzgfakiLVOrPGp6yKR5baAL7P%2FXcihA924rQmnF7NR%2FJ%2BpEpCIiMVFAg6u8GP6ZbCW5jjscm%2Fp05cn%2Fi461AZvY1aoxDjOwGJnq3lRpA6O6diEhB4caf9aoQtODE5Vp1kepApT3gy%2BO2S8EPrTgXq6A89AstoggmNVfGuucEsnqZgJ%2FWsa9f24lp5HAoIl0aEtxKy8knuD01oQLETJmGhzbISDUTT90QnAvrsMFg%2BeRipZndMveEjDe6mjlKjP3zZ8GwqU73dISwEULGXwfagD2sbVy%2B4922J0rRWPBGpucMDqZ7On5lzGw2YLZ34Sne%2BKIOD83Npvkg7HfZOWnf7av%2BVXSs0UcLvZCYXv3aYyhW%2FmuIjY7JVRMCM67fi4XfC2%2B2TyTw3%2FRH08VLGskLNzJu9ESpU%2Fz9miK15u5fVWqOvjhKQZGRfeF18p1BC5VLqO2%2BcIcUgriwKxyVcqfPYC9zHhEnnOVX%2BMpzZgwwKB5fm6mlZ7bVefgxGnI6qW0t%2BxfEUuAcw3dvupLFmwKxdf6SaXGOhLKTTAITZeTCRnilLtx0bcYFC8DHn%2BVkjertISCrYwATZ40sBZcVO4RqCFW%2FaC41N2JiZkWUjyqHgb6Xiuex0MOvLr8cGOqUBecsLPoIoKLwNHheHtrfJ7YvqPj3Z7qqtoziWuA%2FrNn2xOZ8ENKCgt6jiEQ0Al02aCYqE80uzlyuqGJeDP%2B7nKExOYS4Oa4v5IaBRe31gO5tUpFUeCL%2BFethUsOtwACnTPspFOvnnJJS1fl%2FRYXI8%2Bey70CUpmp775eG7zp9%2FspqmcZ2hT%2FrndPUQqaqCzT1sblLRXrBpP2cCMr73QJaBZfTlP%2F4c&X-Amz-Signature=f7464aa4995a18d7d3aa4bafe183940ba224efe89af1783ea28ed5f9a2a0f111&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

