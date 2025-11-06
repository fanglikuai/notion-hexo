---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPMUGB5Q%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICtdJfzrT2y061n2jJ%2F1Gop%2FPzNdejZxn0BiezFipig5AiBSWdgqFjJmvV5RSxxyejZxVhvcLai1Y1yWsYU%2F2cbpEyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhNd6B8uVcgECethLKtwDxvLjbRNR6eYgrFl6ktiMqZpR3GzY1KrgWnblEBcqewrC4ujWeNadx53ppwoOgqa0CNWPaxd6PFQUYP0C4QjJaLNzxrrqHUMarF0eeQrstDMqtZWDKUMJrSRxHLlcUgTCfIpLkqjQ9USHN9IgqioRkr5OTbnBO3io8JX4Pt8WUOnuVbzgeWSfSqnZS4rW18mu%2FgddLM4IH3rLJ6WpqeyYNTq0qnforfojHDtcQ%2FDlhzoQ%2FOYo77UQrbEyvQ6Uszkq%2BJMKYhL66qWHdO7mr%2F24Pn9%2Bbr7d6nQvQCeWQLKguqq9MT48AMm4DWUfjVj9TWvf0ohEbD57OH38qqrAx3KGhOjQ15JZR8kl0JfM%2Fp4gN5zPqbPqhDXr6yx1UiVfnwXaiOtmf%2FYR6XgMNdFhvpBXj2PsSSzAx4WoiLGiK24pXZdIlzl3%2Fi1zeInXsOXBjV1Q59oWY2N8o%2FhKqYAdHC8p%2BWy9E3WYDHMOWPtZsSDe4gMxgELs5Wv7Oha2Xx9YXmwkA7dBoLn0wRD7vf1Byx8KoeYDaykrnHsbDGY%2FTI79X2uedOItryyUNTYZ7h7FrhVzPFP3uIvlIJEHw1tVhgrl8UXmbISMxtN7EFES3AixK7i3h2Qm4rnq%2FfrFdjEw%2BqOyyAY6pgEdHKPk80cq%2BdGkTXJcW5u46SA0gRv4lAJcY8VLD8ECh7ylWfZgw1Ip22Zrh3jkMciOMBY44CwTn4kbkbKjZTICr6tZW04j%2FKtP2Y1VLz6nnp4TJOJ2FAe4EB89yAuN4vkPBpqcTX77jKEYT5sAj9Ia8SLnviw%2BmnTkOdLBo%2BcBCSclbTFT6YaY1fpjd3g2SQQdcIIPx8Zt3%2FDYRSkcrJdKL6VWro6q&X-Amz-Signature=e4bc640bb4f60d1317595f2f0a4967e1dd53a7cfbb218f57a2a561f19db10815&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

