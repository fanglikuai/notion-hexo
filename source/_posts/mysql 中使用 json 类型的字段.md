---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJLHNN4W%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiH%2BD%2FUdpcMkn5GebDH8RmXIXzSEJrEydAMX2ieZDoCgIhAPoMf8fmgo2DFrUHVHfZc%2Blvz1Ts8X4BeJ9dhOw92rowKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4puKQNROmPvpcAAAq3AOfLj8c1XY%2BRJXcudAgHSaRYE%2F%2FEjEs2ELXRoazh0TewKJw%2BEUu9sWab1LbrAW5gTLlXfYtDBS3jVLTE7ggR6kfvwNV1bDuzXynUof%2FDatLNsaN6JiEtz7ZRlCP0yvl13sAzWYBF5ND5eucO043JFhISfkSqETXxSejodJlXgU0I%2FH3bssyCoSTW79BCsx2PgRvT8Hohta4seSV4HRWuT948Z9oet87lUFWqhqW7Qj3wgtSaHtXbt%2BIuaq%2BEaCWMwpRubDdVa0qMhaCB8iHq6%2FNHFQh8pCxRM8uoG1aOb2XTvuiiTdj7pcK1%2FUlAh5VzfMOORb7iwjIBCSO02ZelswZmZnHOQXSs35QuWShXmcFl%2BaaIvzv92XIoriWeav6v44BUB%2BaD7cPk5b443wghVK4%2FZ5gVwCPGaca2C1s6GLcPljebEvTIQL%2BGjRUkYe%2F6SD7Nf43StvVdwtYBIvYIiYHGLvGMzdpDlSBCw2t1MUzWPzxd7WUPWG9wKf88zilV5fWywZ1RH9IDIWrlN5Ejdt7KFEkZFshB7mhUYPbV7HWqIPoTOpj2F32dEfHqeCca7nMDtncz3%2FMp1p%2BPNIiRybv3XutU5e1ioijSuSKdTriGo7goPT1m9USXmXUWjCCurDIBjqkAcWWVFT33Fp1ApYi9sRS2Gr6LWJfwiMXmgaNUsOY6IGusL%2Fxp1PfZSwWFat5zv5u6gMfAt1advu%2BdrSWFP0%2FqXv1MbCwkSS03%2Bvs%2BIOvnFL5oNNr%2FVUt8dzB%2BwIzyIG44TcDSYEAdY4eBiKRBAEzfosFP9cYmHnDcFcCl17FOq45xUHlJ69AAOG4%2FYzOFJdXdsr%2F9%2FArL3NG%2F6LY%2F%2BTNV7cda1nK&X-Amz-Signature=e57704f141c6973307f42d3667ab1c11a7820058c4527066c0bd2d5f21efc0fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

