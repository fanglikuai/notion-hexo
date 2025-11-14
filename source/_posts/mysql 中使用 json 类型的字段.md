---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYOLKFHL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T200131Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD864pbWMPPHWYsJng%2BdXl4lLysMYfciPnlJzPG2KEBmQIhAJvPj%2FlCszgF8aXU%2FXM%2BmBUC8aGUxkRzt9%2Bao1sIXhagKv8DCG0QABoMNjM3NDIzMTgzODA1IgzY7ziP1ojNZQI%2FGaEq3AP5G2KCrzFs%2BP3HJvJY4sAgbQo7r0j6j8auqQ5FnzZOwQ0JBTT2%2Ft88BcdDBCb%2BjS2rhP6VVzDeiMNGoPHFs5q1gTZvyWJz5ZYnk9GiJ1HEEqH%2BYXrx45u%2BKiKGKL6ksgntcdx%2FjHP0usohM8QTa9S9ApvCX860IoYQA8i0IVLacZo7%2F53mJCM7eynsbvo86YqxW3SxebvCvlYyBIJJ1ye0PZtdpuK%2Fn%2BUPmGXSSg%2BG8Vpx%2Bqw%2Bq5t0EDXaMKAMYql29Lb9RuRptYQE%2F%2BLagr8QkcbrmZtFzEMjLezopSfQXxuWUU4pIW%2FWV86QzhWMucSRtR1%2FEf0TN7ZY54JmvHrcTO4ftU%2Ft3sZZt9znAz1yXptJrEsm5olSBtETNbBu20dD3T9l%2FW0OiEjXiLw%2FjF4XiAS99wLFmNXipHIpjb9Q8iljI0pPzhLMpVt1FE0lj4AmpllH8IEBa9CsnTPAuT9vdsHz9hwcypKXTKEOfzQtP1pR2nQzzTQujObXM2eBegKUTCNPwne2SbU0Y0zdtY%2FlGR%2FbKx6Zb1Pt9kUkH5qWQM8KQ9fQO40Zw6MzN0AOWMN5nxa6tS5c4hrPyVhj15tA2KfRy6a4YQm5INFBwYfjN5DIWK6Iql23vSXJejDOkd7IBjqkAevzTED%2B%2BRKDgv7Bkrl%2FZgfCOSqoEufUGqPDhy198U23OkQbwmyxJOysKFR6I36bw9pqyGFUX%2B9TM35Ml%2F04oueT6hsYyxg7i8pM0WYoNWYsIjqAHTGKx1WtY1sdHQMMdPH1Iiq9sh1xGXYVBeynMa9OW%2BG6rpm2FjJq8vjKMHkiTRdZquwW4xrjLwHJULwQmqHD3RaDDmtnYqKdlr%2BJ9Ob0IjxK&X-Amz-Signature=23d7c3cf949a4e81be29992f5f6a827420588b0e3a19a3ed974cd98f97bc1bf0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

