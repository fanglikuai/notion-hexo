---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RK5O3DC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIQCuzTtM8KAA9xsNK38bN7lWWuLP8S%2FqGTgjFA5Wm8xD2wIgGomdxEGcBujJTzv%2FvtUJZ2Vp042PVMBQFZdK1OLSjmAq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAbXDGi2WuJ6PqW9oCrcA5b%2B1sjec5c6AC9pIA%2BK06ZsCzP1vEjm0oAoHi3nEo5BmEdetUCXr7rjJRqRg0BknK4z0KjAtxWxgSoZ8rBKJP6t9aRPcX2%2BCaMM%2BERxl1YyXKyq0WEGOLMtrbndiKnxIgpRrhbjTmF3yd%2BEzOEx4i0dwAATcxyaLISrVtLh%2FUh8DoXgEvRcKatRF3cqcNljoyD0Iqss6AH3QUzVy57nTETlnw0tQzDPEP78%2FXImKVNSVpf3ETJtfiDZit%2FXfFQUoYseuhFLZu1MkUcnwhLyAeAFA4t%2B6JxwBOR93CptjqtCU2aIz8tvi%2FmqNlJiNq%2F1rsomq%2F3QxbnIxaBeZ0gG0oAg6dBcryVraGb1UNxDBXbrNqeDouqNMgmDnavJJb8D5dhiExGvN%2BuaaK%2FPglOpPbiFnToKHUmIsnvR%2FM0DeaGcA9u7Wt%2F%2BY0DCSXgxj%2BRurDfTci4zyO%2BPqLDF97pX4ZYXiOUGYtveHnGXNNrWvYcCVRl%2F88%2FbLNb1DZmtse%2F4IwmhIqN1J4Q6A2NbmuIbQ3%2FN80BCUzlAlsxJ6dUx38v2cE9Xrcz9KNDS7HyFVRUzwEbIlJQ9PipHvTb%2B6ao4h4NojmLcXAxkQx9MAxLWdPAW1O0h7S1v1aXLuTOsMJi6jMkGOqUBcUNJehaFKbZAZ6Gmn7bfP8NzCx%2FsZ8IAGTzmzTGrwNbjzVCMvU6qp41co4Ay8yVSck3E0DAkHYKlshkFvH5cbGhiF%2BokZN5HT%2FVTwo%2F%2FUjw1OJV6JkoK7x1NQdriWyhoU3QOxSUM0U3rdZbI12DA2S5B7wlljtOB5tzqkmfpai4LXW3MF8jh0viMDQwlB7IcRcG4mHEDEOSpEXVwjj2VFXTUoOB3&X-Amz-Signature=e191fed537e090d9e7394d6d6e7150504a66e7e57b257e2d4ad9fde2bb68d50b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

