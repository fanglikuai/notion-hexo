---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMTJAVMB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T050222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjTEbos6RK6c5UqPFeUM3nthS6es0mrVpTAQ3hTHYhmwIhAJ8%2B04bBanuuiCc1q%2BjZ1956oaDD0NoCzt4J0az9UgOPKv8DCCUQABoMNjM3NDIzMTgzODA1IgxznXBwV%2BSuRe37dR8q3AMRqA9O4vnK54b1uZc5kINoO9SodAjKCCOYGbtQPwwAOkhW8p2GVjLqeEr7QBCERVHy8EqvYe8iVDstscCvBm5pw2GRLSJumXo8K0JEc0LEtCU%2FZDdcMWGwtQUbqSiJg5IGyegF3YfN%2B9aHeEX%2Blx%2BDp%2FRkiWhloIuIoRSi9mTnOSWG3aDP5AgoUBCHPwZcgV899c4t0m%2FU1PoP1Fp99Kbhmdt0ozp28gzieIS3nA%2FfHNoe5ni98vneStK2Ku93hOvQXsRHtVMjh2VmIiyfKUQWtiqllC8npkvvyiMMZ6qLO8ynNAknLD0pYWkAJ3Wsd43WicqrAfzNVJkxjwQ%2B%2BoYjvuIacm3CDMa5HyacNh4AZgdQuuwMNuyDtE7Yqo7X30VILx5q73eG9b%2F1i1%2Fp3j3R7XlPRAFo8nbMmk73G3%2FxQuC69wQBycIe%2Fgf9teDx4NROfQoTT%2FsEMjDbIBVYxbb7nCZRSYaMXfhxgPWzeMgLVYZeAdXynkTcQQUWJaoZCeCVNEHtdwLC7lKZFEpm%2BjEmONXxpK5bTRUf%2BFogvb6xZmqvbCplVmhpARaAOe1LxmHeLogAc8CNd5aKb%2B%2BlKp%2BiX1j3i3AMaRSVVEVM8DAAp4bEYiGm7QM59%2BN8zjCrk8PGBjqkAd1ICgrQut%2FCPMoBkH8ufNdt5wdDPOfwp6aTUEireLC3I6bFzfpFjOHCT4fPycsggInQgc3DpAMOigeCKSMtaWaxBtPTr%2FP6QXNzfxJuY2a58bHRdpv5Ox%2Biesvn773JmiW%2BNyafCoanuJAOCHxNsKKHjLKT6LXVH72v5IkNAoLH5%2BD6JQ5M8jZNAEU9qAT50kc53hM1642ggJM01G70D46rjePR&X-Amz-Signature=b2d897f889c4871e3dcd2ee469f5cc5bca8f3ac4922c32511bec463e64501e10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

