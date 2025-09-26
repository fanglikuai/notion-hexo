---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVJ7DYS%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQDfg8VLiDvFh5IzCtI61Lv81oiWIEILD%2Bj8Fm%2BBZevhMgIhAKzckQ2KeIYkhuxbCI3ztUKgR90vCzF2odRfS%2BrbfFnNKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBDcOtTvJZp0Sri%2FEq3AMJgiU6dOt0c%2FT6rHb7aB4yqK44aGGzLiM0oHjrRmKkxs3WPDHO4B5chqXrM9wdUtySxaEbxVRWTaNiQNsenlhoRT%2FUmVYT8jXjDTU8YuFiXlRNXFJbFU821ZCy4%2BCRFIxJSgistCq%2BP0EZd0NTVp2OgwnZQMbULK8i8FqOmTCrwtlH8%2Bp4woa0W6yU2TnmQP8FYw%2FgGEYgajrbcgwrI%2FDy%2Blq2JncSO1FOanXrAzQKJ1ZZe5njU3Dr063yEW%2BMYaRTRwix3o0kgyZRDarSqyWdrdU6seA%2BnBjuP%2BeKfYo%2BFwAqxdTwFIvSAWpEW%2BqIR2Mvr1tpMC%2F8aHlxhsBeGX2mut1lET4CvZvqQOlLDi67iYlajleLWqUso9wN7rnixZuAIWdK24MuP2YN2JogdwHmnDMsV5jEePRiwkapSuSf%2BcizHs80UZjGN6DHdpvywD8m90%2B0u5a4Qp8npVtcirJ1IHhdgvo1kom%2FviKDPuAQg7HRH%2BxqkFJQh55MW7txvhB3eUKv0Br7gt069D0unm3sXY5v0ZED6XBOIr%2BFLdXQOlGVZsvKsrVW6EuRRLcR8%2FhhnPg4lxN4Au8xnpKid%2FYHHFBE2TP59BfQwOQLImhxKpZ9ZurtuEE%2BDX1H%2BDCy2tvGBjqkAbhXefOUCmUzQXNFKeB9I4AsZpFyh4t9EAhl0h9U5jT69%2F6IW437IIho3bSK%2BZycpFwIPhyozp5RIznI%2BbI7nLylzggogamhBpVT4f%2BNfhT8XgzGWFuyfBbAVz0R1pFaOy35FXC4hCSvuECb58DvidOdpDHZz4yYozwyHE40xZKXi5eGMFxsXABmBPq%2BaMXtKeQcWQhHIoP5toBbQzVyRpBcKSDk&X-Amz-Signature=cbd288d9bfeae4772f744562d25253978df4063accc513d2ec557a1fc83a54f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

