---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2AWMEAW%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAmDtTT07cXlqwoarn2p%2B5A3fXa%2FMQQl6QBtfvshzJlzAiEAgDICvF2HG1FzQbrqIQ5kEUvkFKWZEXq8BbNKTMhpkCUqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBmVNt8OR7Bp4SBYNircA5sgsxX%2BOkzwFNt5mhh9RC1hVbCEIdd1v5VR999K1YXw2%2BZluo3lbDpRHpz6Aj7ctL85%2F3UwsD8NvaxnAld1EEuLzyzpisp7J2BcvZYCMmjozljpsdDNQZ6VV6a1%2Fl8qdyFFoi2jy%2BU3%2F5scMN%2Fo1v9fvYOhyD1PU4UlYapi0N8L8t%2BUYR9W61Idp0b9YK9zVDGpkVdr%2BkghvKLMAi8h4mlPttdatSqf1iTc0ALnFwnvCpdFk6PXNTbGzPpyXuOZiLN5r%2Byi%2BEzGTfF7kKtfR23cyYQ8cfa2xbMgwd3yJaAZ%2FmMKHqj5k8m12hGj4DkE0bB6UJZOk9H0O3fLPrfcICectAV%2FGxOg1GkIb5n16jXK8GPe04FJeFERiN7rfpOWfcK6YJ1OeQ5CzO9m3do8xQVo5qua83OChVWjqoDYybJgAI%2B1l2MrB7ICGeQYkp52cJ9MkLZHHwkW40GaPaBpQ0DmMvU37ez%2F4PSoNqCkdQM%2BVAD7nyDBg9QiImrLzExAT6tYtOMHGHpQ1DPIloUFKBzTNPGPmwWbTGEvblQ8ud8YdUGmgtYtgfCw%2FoCwRXn7hBuEjih9X77M%2Fieaf6yGMkhZl2TCx4naBI9SwMZ%2BmwDVkWaT2JZgnrXjE2VvMMO7pMkGOqUBop7mRWU9q6z7H81T15LIyNWfv%2BLj%2FtNBEsFadPpQhCqR1qQUXokGMIrPgIilC%2F5xswsgdsHSsKldZ3R0rCNwnrr7T8t3BzCciKpa5UGqca32b4SEfqV7DYcRVfkHMfWoJVRyhi7PeI70jrx0UoAadpn5%2BpUZmKnpp6J6E0Ky6D5%2F0TWRinxXnKqoGJt9DEVmkEDWDS6rvRZU0wIojwG4UIaV1rK1&X-Amz-Signature=c36bdc9773275b7dd18f76e1bffc429e3a3bb1cb5adfc183efef77ff7cbe9c1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

