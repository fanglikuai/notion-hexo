---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WODLBJSX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDr%2BvHVJ6j5GFuHrmLZEJfTlqCz0ZBrXI%2F%2FhG6KMG0bQAiBYSJS5L%2BC5T3qkVnVu0bN1VMtQwVfuAENe3Phf9Xnjlyr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIMj%2FaIE9%2BB8IbWeo%2FvKtwDTlMXmRYH%2B%2BF%2BzU9HMFnzrTaKeBp7cdI4Sj5koXtRF5a1P3mNwbw2hrg%2FjantSGE%2FXC6%2FS936%2BBmG5uX79avafke1z4l0iIyeDrFa4WjPOZMKorymRQnwckCvCI%2FIBM3dexugGr9GH9s20TV9rNUjhDjAaic3viCOc0zw8yXm1Mj9k29SK0utvCSN0sEfdFcb8I87wn7T32WCp8NwmfN4Gf%2BOrj73CUbtr6abXQTHU27UFskBpo0MImjALO4FUCcp7PpZyUAVjYKPR%2FbpDNKOAwvmbNMPe%2FnzmbW4OUwOCfwXQF9D0Hpw0KdUrs%2F36sEgSHiXHerWUvK5KCeHA9UvS6vJSIR0ohFmHzlAY42gPu9elIB5XIFZJ8deuCLuzictZMO%2Btedhtqp57fOmsD9xq1Ov4IM7V6lJVBoG5M6UmrkTMYN54djsBCbzMSG%2BnncIyrzdzT5sQZFKF2aAJBBYPfE68gRYWjKKZXShhe9d51%2F4GGCwHqfJc9r5p%2BKpY2Z7vS8jptahsbyIhyNX7tBgrahJ2SGF6wKgJLi7bzqqa1S3XTMwrbDz5EDLkpElG%2BSTciq4VG8w%2BkcrDYdoW%2B9APmnjT6gBX%2FQv9qYNexSZIDbjLOk1OD5DM1vnnqowpZC1xwY6pgFs0ZIS%2FtCPAnJN6p8I28ofS8KgS%2BmbE1%2FPhFjapCdaKdvnmF0X%2BZJwfWqxefd4lp85LfeBYfGWMf2qEKmMivO4SQUFRnI7kSiFRrC%2B51w9vDDoZKh%2Fa1X4qzYapTpKH3oSH9%2BtBq0%2BazgJFYvHcrhGbvXp%2B24xv2UUBId%2FoD7G9%2Bn6eFJ1lDiDTClzd4wM0%2B%2FvuGHDcURzdETNzHB4M%2FIcmH3LnX8O&X-Amz-Signature=1664930903bc9db930a6ceeb3b48c406de371716e3e27cd923cdc7f81aafb967&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

