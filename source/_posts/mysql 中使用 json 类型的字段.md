---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DRPQECC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDMA%2FC17zXB9zsFEcmPIij8eiZSiU49PCZu36v6vvNCdAiAc%2BGbETqIHJcQZ8MqNSAVwu2jHzjmq%2BK%2Bc9MDntvfcFSr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMQTHzZv1jVM4MYX3HKtwDf2NR7fXk4sngC96224Qwa6ZLCBfUKK02KDlkZi7qHnvRdXZWa%2BLs7P40JJ7lbj8yNWf%2F0RoZilcFkh13a6Tpp1xcxM%2Biv52%2BHCFkCakkLzOuE8GajzKAYyMaDO4sgEWosd%2Feg0YBa6B15AqQ6xnlqpnqawAeYe7x5R5Z%2BMfDToegOR5QNTICQwOIiOxcBMWwI%2FIhZ286gQmWrWqZrn%2Biv82PwExvVTUnwKMAVS4VtxmVIgUpoTO6UGwyYl7bjFFFQWHsS0MPI3hedxyeMPugTyxvMf7wDDfX%2FeYo%2FEI3P5m8heu3w5mQrq32RbmHDy1yNmyKZOoHMpENnPzKB7f8O2u0FKwHquQ5dGJBr%2BYNYQmMAx%2F08gewMchUkyw765A8j8%2Fh%2Bc1Qs5sZ0ODATb61cw%2BqNaMSR%2FzVXkmnSSmK01pFTz%2Fv2nBm5ZPBN1yu7FwLCOUZtvUb6OtT5qfmb8noRBqfXIKRH6brcgfvISWdFrkvlleIgPfKo6WFSD8ieEfuWoTJOBNiDj89VB0Xf0QSMbeY8C2ZDx%2B%2BBHBGLQtUzyXvMjCllFWef7wdXnULbXKugQ7eaibjbiR6xOnu8WO%2Fcednb4b%2F86zuKjGpAAzYTKOcOUHmiTf8HASV2G8w2rO1xwY6pgE1E6a3VjTE9fOfZ8zJlD6cUUcw%2FVdH6u2vbnS2iWEC6yRwrn9ayaPJ0AP3akgEHRM3akIGmSuKSga%2FA5%2B8ZGpOilnHMc2yQP%2BkGyECvCsvswz%2BZASuEBPuA6RuFduNJx6zQQdoA3S8ls7sQmz2TpEpl8FkzgVnjQOHN7GA3WS4957T4TXMspoRuO7DGv4Y7qMEzpbJa5%2FacoApQ6wjMro775d7naA2&X-Amz-Signature=6895a0b19d62ddb760123381608b364f79879e5507762e59b24097d82eacbaad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

