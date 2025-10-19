---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T4KLVXD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJIMEYCIQCFYsARMgoPgFt9D9IoX7EWzDzlUKTzG1afi0G%2FMmag%2FwIhAOWDhub73RS0iXc7bJGdA6sOMh%2FcIeioML%2F6zun033ouKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzbGhpGP84Bc6KIYPIq3APwrT%2F1rc5bnDSKdrEVj9cvugrqyoiI3x9pVczjQ3ILhbOJXFgpKkX4ozxNauBER4JxW3ar3NKlLuP28CYIot3V%2FOp3rD0uHCoCWQjz6hVUrUeqdIdVQDE55jwnQcZavgdoVwrBcZMRXxdh0JMnnbxH2oaSjKqvt88oW0TEdAg%2Bw1NJt7LMR4XQQhW51T8Wxx9fEji6LE094GqS73gRo1W6TUuXFdf%2FghLM1m7jJJMTBIotl57TclWSEVOgeA0L632gh4oC%2BaenttURwoW83cuEp21MVcMX1v6AX%2Bp%2FBCE9lfjvXo0dzNW8SAdA78kXFNwJRqNW%2FoBNI4PupzRiY4M9AcyM1n11cdW8L5xdF7g9KL%2BKXJnk6lp%2F1jNFLupWWMk5eVJN%2FiNb54ezijt%2BFHmZ9AlifvUarr7qdjP%2B7H3iAiwQ4eg1TBmHFRLdh4O%2BSU%2B13LuRdHx2WPBXs%2Fi8Us4Z97YhzxHzz%2BCHhiYMpZA5%2F57FlYeCwF%2Bcuq80atc2onpEurghBnNZtBf%2FgY5XCy5gz3QbcSMjmSZu3%2FNIfqWS1hov0guZmfrn%2BOdmE5XPjFwn7XK5FBrcyPXfuI4Avm9z6%2F3Kez39%2F8Z9N7KyEZHAgrYqxC1N4qp0bHUw3DC6jNLHBjqkAUN5vyLVqqK0BfaXKNwroIsBJJLxeqqYibGshdxEcXj3kj1GSyVxdlZ8BD8tBrEIrRO7LqvsFxwHq1QBFUzjX2EFcLObeLrO9sTnbTTWKIyCEn3i4ImWBfLw9utgWLAb%2BFtQNcOoKsHfjMC3T1%2FBVB5bW%2F86SwhRd7Rrm1ihPO385lgU9FeCzjt7fcXf2TJ%2F2sMo6j0P5fkFnokyG9FAHHbAH5JV&X-Amz-Signature=0ac18b648824da310488ccc629c8f9f69438b5d1437e0d2bb442fb51f7e2dc0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

