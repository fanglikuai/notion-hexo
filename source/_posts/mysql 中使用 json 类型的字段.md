---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WOOPCLKX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn0M5trbXl5Ye5RqKyIr0921Cxbzlmfi8znaHav8oC4gIgWrXT%2F1wtDSTtjhcajn7UUkp0GAsmfl5T4ARCgP4iubwqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL6zT%2BDxVwFWsqLDbyrcA6UcA7QDstEiDC572QjeP24kA5O8MXGmpzFcnUVco7XpHzeId8S2QWKn3H8bKhczRBFumBMaWV2C6w5l5StlwS1gidX1oVFDBoA8xJiOYpINa7RvtphJFPuo9JwQQ49Sis0Buqruz1LrB%2FS6lQ2FwD0lrDpv8Wnc8QFR2%2FFagexsvg7utCBPTooYPEUObv3z0l%2BxoKzI6K4C4bb%2BDAoJa2eYL2%2BIORWXWehjHgUsa01oBL%2FAQYdPzXqppW1A7sfwoRfkyX97yqrHOPIn8Otgv0mRe1cv%2Bxhq2pGPaIV8j8hyyVDd65uNO2E1MqS7pmUC2VyXz8WaeZV6js5u82rtRzDpWkRvxdw5LrwAysBoIfKH8KXwmOoInfj%2BHlrxqx0rSn1uHy3NxStHdSBb6CeCoEItV59C2q4ZDSYonpgDRmYrrbI%2F55n0szjIz6LXbCafPk8QO%2BnlU4gPtAc4f%2FQplpUZsqF5azjR1DlchhVTnr3SGLPoV%2FvGDQomLcRYHCORSX301e1rBAPZY9OeerNqzcdSHXruaoDEue3U2%2FnRMrMgrn%2BKCGA%2FugF5gLHhYdvDz4tZTocoCU7nUEbiO364MHyay%2FZOwoWHloPMWci8aLYEicQRUiuQ%2FY9ivr1qMKGm7cgGOqUBHht7RCJdRH9hFAiUaOoLlIKm9mRSxHnw3kDd8O0Bl5dfg48%2BsGHO%2BmjJesy8vdt%2BDSY7t4QQFOusaTvLIFyWy%2FTXMbL%2BsmyJl8Ay3iawN9UZwO98d%2BPmpZcrzEXFzNkVa%2Fe%2FD0lCKT7F99e85Zr6VAjl%2B561O%2F4tzYRxeCG3o927ScdT%2BOLqscqT1pJcXWxrYj0ID0AZb3SrqsOU9TelXzeDAaMD&X-Amz-Signature=a58389877a04be4ca4e2029ad8f75ea6bdd877af1914687052f0a14c7846962a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

