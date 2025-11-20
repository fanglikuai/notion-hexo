---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VMG6SDB%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJIMEYCIQDrjbSXkx4nOKv7oAjiwRidAlHh3HnUki43SVVKrYwODQIhAJ4C%2FHLAvSzFcrNDfV1mdl6iH4M9K2xCqsGYGntRhN6WKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvpMW07SF5UnqqqEEq3ANTCVhtiZ3w%2BEOufWxDP%2FHi0T7QfcAeQcLe33lGrpB42yn%2FhiYPShRDZOzRJRVqj2tfKiS0Qfn8lBJDn46lhwRvYpJJ0oe8knZtpoEvLP8wInrtWGjh0OHAsevw0XEbn7X9Vsh8C%2FgHEWzSAvxecOb%2FfcikIoWPXBfo2z76KilbUk5rdsHTRea11JXfXI0Uwa%2B2qfbXvORG3bJgSUl2TxxjTB6b9zvEUeJSKDNuop5lvOYxUacKSt8oegAaLto5WbelOFgBlp4Dh6w56u8LCEyrdQT%2BlmnTx5NB7%2Bk7j4woJZu9KUCytKmdga0zTbXdLBcQv2YD%2BoWBT14zDWZk1O9mzs9%2Fj2LeR8SjILpjE05SBcZ%2FX7kTblVPGC4BT7v5A5nEQE1HlTdjphqQFz3rxDHbqzs5McbUk1rRz19i%2FhdMTtvwSr4OEJdKcKeb%2B%2BJAEN723vPqVoVfdCvJj4mKzZkG5K%2FueENGPfsouvZh5rAsuyrW%2FtoV3kiv0M5dJFS064bw7cb4bHAm0oOC2z9gzx%2FfdM5rSPQ5IVbLIqiBBHDyXB4G%2Fxum2BN0zKyiotps8k%2BuZ7Mv9%2BGVbvT4oZDmOsubMqg%2BwzZigaQ6i%2F96ZdLGnXaLqSdwwYVaYSsz9DDip%2FzIBjqkAZBf7wVPzZSBnL3%2Fg98YInf0wvHdauAGkwIwM4p%2Bthfq9Bfo0ICjlJG4WXGxs7Z6PKRbn2pIg%2BPlK5%2B%2B%2FtFB5YWCHw1Mt4yXvG2U0J%2BSCkenbP4pKx1OcZqiYl8U0jzsXyaO6J6y5sEdkN2dRx0EX6Hxx7swez7CMFNqRdaLbOFYzV84SFLtRxXDRAtNCWXwVOBv1o3KpylTNY6ef9AMLoRwYnU2&X-Amz-Signature=61b94d9f13aebc0ccdfc698a20fcc58800e253d4c19fc73e643a212004e40fe4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

