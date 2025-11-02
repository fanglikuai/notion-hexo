---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FZDBIPZ%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQClnkBsIAROV1CR9JyjHhdLBkeGZPWVNBORP1tA2o9foQIhAPrJbX3uNpP7MDrIlWozcmeDdGKHCYeLg5fVW8LqzBsuKv8DCD4QABoMNjM3NDIzMTgzODA1IgwkjuNbBhnVwwWOf%2F8q3APNlkgaSeiLdJ%2FxXWoV4VOVqp5sLy32SgRevm36XLjHr6%2BWmU3WfB63LErypGxgDPrxWQXHg3FUdfdjSY9240Fdq7%2FKLd%2BB0n%2Bpe%2BwK3FB%2FXETv%2FKpLvrhF4YGm3pMF9LylJwddsOh0iTEV0mEu7E9KHojLQ2lT5BzcfUfHvuwcIBdpNgY3VwRqfq3xaUCRsMqpsHkVPUh7pOS07Uru3LIWSNIktSscb%2BJLG%2Fp1QEwjBhqBIP%2FRcELiubX%2Fx5Tsl3P7MLXWsUfbfdAaf3HvM3UL6ebz30sFQSRLAbLwxWlW0%2FzFMddWB0%2F%2FIj706O%2B%2F6trcQ%2FjaFUjzFJuzgCWHe%2BTLM1XOZZezxIvyAQayueWaLCVKQWelKgzMDTBmRz5wQ%2FbU6htKBWtB4arsJ2Ar2IP4ctcLhR%2FA7oAf%2FMZm44eP0UjhCGmQMlCd1dPDmZh4tY0rKXSRSTJwjPb2OqUgtbSUwl3Pol2OziT9Yg4tlzlZX2skyrED2LKADwRMe80KMLYSNMZNeXgxyUmS3dYmpPxbLaoRTlWSC2H8ce9n27f8Aj3s9w1b9N%2FGt5F1b5Wjv6wUmRu38UYvY44wfW6mOCb1L0gp1wVycUaax0JHjBa5LOd6eymvCGrvzkKT%2FDDf05vIBjqkAXjK%2BjAOBaMxYLNmZmmFpfcZ3qbMO7RTmwyDKJhZv7aClfcKnLC1i5bDQvNquG29u3UrtwJEdjMxuDdChPjtL2cxX1KV6oCIN6HrDQpJbynQq4xis3BzCU%2FZVR2z2VTMfL%2F6ykbZrxP1x1u5LXPTr6Yd1K656QsuUbKpYRP8xnTJhZ7oqc5NmVFQEIH37PQLzHPpMuH9KgGCt9GUW53FSPfoUnBs&X-Amz-Signature=c43780dfc46d2667661fa087716acc8d94f2c233acc77c6f674493b6677c13c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

