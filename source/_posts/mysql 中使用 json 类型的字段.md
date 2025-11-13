---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662R2ABSHI%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T160132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcOvJzkkZSsW69HbyAWPiw3QWNvmSD8sTE9e%2F0nTX4JwIgajVS6gVyG3yzm%2F31%2BDT1x6ckKajs7JTzX5OHDbXlfvcq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPKAIBsaauc18sO6zyrcA4DSWwLMuyDejIV6Ql4I3jrPbYa5cw6VKt4rsb7iVVPRmXx8i1E6Ag4sDvK5a5YyoLLnUHg%2BrVywX30hgiSLTIP6XBBv96Bs4a8sXbLNUbU4Ng8YtmVFX%2FKWuWKtlBFMnzD6XwVx6SfiuAwuY770MVN3pPKILY4XGe3Na8HwywpFO5RvUmGq6UJnCxnSISQSQexQmExfNvVf1T0%2BkrZsMyiGsgD7iv2Ye%2BBbAsOn2HU%2FjvXpEbR2TnFraLmUWr04WCKWc4L5VSbXp8kI5Tn1wbBODQpZ%2FYEWhO9gVlAvwSM14ieiVhPd9kLEEgpMlap%2B2%2BhuHPkylo2zaPSVSXEa%2BBiWTpugtzpGOI4DYmcKJPrzaT0uRT%2BUoxOnqHIxT1bLHVI1Fl4xEvDBvr9qtKwnPL4HPSSSReHwj9KKTzDVwHlKZM8z3TrGxwLYKb%2FiY%2FIZ0Qq4ru3LkF4%2B1FB5EXjb51CUWnFfZHuW5q1lnOp1%2F2uqu5V5lWT%2FnzTh9FXNogoMZJE0OnZZ4pWAonSAu6jGK%2BLOJdgTFt8yLSruB%2FpqF07GR8pyzkJSzeD9diOOLEz6T45TZE82ErsErA25wupa9MAlOC9QNJ0Yb3Fd%2F8uFCYIUH%2BdonnXfNuEiYF27MNnf18gGOqUB9LU6VE80cCUlQySdf9yYrxK0A6Yi26Njh7QInBxyNc8KyPIq0lcg1vq9rLDDOTxyUm0HpfBSG5x3c7kLEY6IKjK9FXrLrqOa4dzgTFpX7HoHh4aAwP7AG4kT8QQNsNaCYODueROt3VYcT%2BEqWucqsk0Ta6Dl7IQhNoYfWDcZEAYDAgmgvQTHWvAJL%2BHAaiHtzGDaFPcHcfuBpSdbmFRxF%2FUWWer0&X-Amz-Signature=d6f2dca185d8a793364584e84306bef1b61cf695163e75bb62ba98a4dac29712&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

