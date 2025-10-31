---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3CZYFVH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDhuynwHjY7bQ6GFwwy333G8zSTMzZrb%2FW%2FwUZVEzwsngIgcU4WTo7TooaZ%2FlJAKfD0WwHFZ%2BUq9Clx37R5b8A7PCEqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGSmHzM7IyKdcoCKsSrcA88s6jkSJDh%2FtTj92CVqO5p7untDopRV2s34IsWZWg7TW3SIIfx8jPuyFsI0%2BBeNAAm29v0l%2BUebr6TKMXmTfhG8tecrGMXT3Tq%2BUojhz1U1ybgys4HIfMq1uvpJujdeE62CYok3XAQTxD8UHImYLTiGRqCjDyEGdkO03oSyYqqKC3Ew0KCx5zKo%2BNrQEQaoaqx5B6c4kAdY5vH%2FOzXK2A%2B0HWve7Pmgm4KoWhGUSQ38%2Bl%2BsaNc7RR7%2BQjkoPN5TYZgMoZuiPTpp3A6H0DBZqc%2F4RNozESqi0mgjFaXQDt5Eda6wztc9Rb3kcqhsambaArJBxivKAUKCLmdUD0AHCgVKGA0Zd7E9sA9sBntzdXJoNwCzMydgKdDBBHeLktRiAWPTakS%2BuVRbrFoWMCPyJDFW8MU0s%2FhhWYkRqzqpa9DhHvbVltUFzXJ2%2BcJKn92QWW%2BYBpHFebORg8sSOkRQ6KMwnRK4CE8rcMhS5veXjGASMoVwmqSrmuYMfSEqeSHi%2BM6xfbsGALwd6AS1Yc66YZAgpoVNDZItELiYUcaGaB0U8mIO0%2BUdw0Tquil5ruBKWG%2BB003PDSqcGFneX%2B2ByMrBKY%2BeYqzFVIE2Er7iU%2FmC9YrdxLzXAodXLJQfMOqLkcgGOqUB5dCJZFWmBXApG0t%2B%2FmrW1EH%2B%2FnflY33CjSiv7UBq9%2Bf72em1WZjdXgMKlDtdQKP9t2w1nLdTLfvQO1KiSFxO%2BiQ7ZHXP6Wnp9f4IzO7ANjSM6ocniEKbJWV6GPooL0cv6hZmxvLc0qlXFdlbi7uXOw2fgg69RL7fB3W52UQZHm914C5x8ufweXWA7pPqFW%2FjhI8o7I31nkaopacVs5Ah%2Bjp%2FEjis&X-Amz-Signature=2381357099e7106ee6c3539f8096987599d2cbdd54a3f38024e14e5bd7b501eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

