---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBQKXZFE%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICIJOhYeLS9GbDJtYTbjvCpO2mXNpuDs7qFAehYCJq3%2FAiArVh0ps3CZ69bn87Xx5IYhXrtfPbvFxofPwH%2FAEqaa9Sr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMrmqvjfFb7EbRIDLwKtwD00Zap3tvGCipHMkYq%2B3roC2RX7JED9Ogt2J%2F0i7Hh4kAFojb1PcBnC%2Fz6Gs%2BAm0obuDEqpz%2B6hFOQKMNAWvcoe5QM5PYJGpNG6GfloCrLok1%2Bo8yX7vnRLbDFqW0yEgUSO%2Bw5OFCxXDLfaacwPFXY663HUa5Sa83OWtXBgXW6Qaqb%2FjBLQRUNVTVztBnlUPGDs1PSN3TLM8qxalYLeW6Hnl8b%2BEIRE2DahEJrAnCy6atxP6RYJ1QfpBAYsN5yR%2BwHZ5xvCCmz4%2BjDMKBepHuX8Qx0jAVVDLODZT3%2BIqJTtkAmTNa320c43jheBFrBhHhtMq5Ds7hG%2Be3MXLRf3Pc5J2Pfo39a9e3zBVhwJDNEX714%2Fo5fKFTVAsjiOmu1OTXAaD8MVapOcL1hz%2FSLJd0dahUTJNiwKyRgYe3aIpFn5zy44X334ZAOXhiTkms88qfz%2FgQ0eEufy1ioAP8dsNRudiGdArObl%2BJgFJXjtqANpKE%2BrDMFjPPpeKBM1ID2Oq1E3QUV2w49upI1S9ktZH7DiCFiwfSJMvjSYAhSO871N79O0kQhtdNMHGLW6iC%2BdegU1dDTJAMwclmRa28301ZgJ%2Fx5XBToB5O9Ih186bgMUzF0iPiitMd2aj1tFkw7%2BnNxgY6pgFEVX8hCBLoRTASCDgWP7qujIjkIMaA6Kj0uHL6jSXM3Z1PHWu7WeTRAoh0mj8DLWBc8xWYdhAejSXK%2BYTBA3pbNZmmukMXQhDMfzo4dOCv8sgINwSafKBqDbtM4z2ArELgxlVsXvr0ZoDKQ1%2FwELl9ltIDjEn0M4KZD%2FaB%2BqFJLsHUqoGS8g3zwishR8bWb6WmnvsdWseeAojC%2BFeLDAikSejmBq%2BF&X-Amz-Signature=b3067ea74956c65687a298aacca52232fff002a93baae6cbc3d3cdf7b03d56ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

