---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YFVVJMG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQC5fY0Poujfea4DpuByQreTwjE8osayFOB%2BrFWVzv6N2QIhAI1tQjniAQowgqJHllflDS%2FZg9p3k49zJGAb47tp9WRNKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxT2QeEfz16Z6BLtRsq3AOsoDmy%2F%2F%2B4vQmAf31QwBRh8iEMjiZ7wBTjyaXrT1fe7t9jSWJntWs%2FsGFeFY9vEUTjB2XhT8pkqs0N4VMSbhd2c4CP%2FoZmYfwXhrVtglP%2BlLNuMdd0wKtucCGKDz0m%2FA9BG%2BcO5i4HEuFSEmELByPwWRLxAQzNzHbD2r71uRYyK8NiHOlkgOgksOa%2FXjAiqelKMEiEFnus10O6aAzHWx5ZITNMck185dJugLW0W2ZQvE5W6zvMjlgrPAk1FpK5Lrc9wAapESbanweE61h9KWAc8MULaOxQ6TXElMmeThTy0sTSmU050Rpze8SfEqDZ%2BvG%2Ba8PzhAd5FrFkeQUrEPXp65r8jnMXi8By2U2939yRZd3UNimUaVFhtrGhI0Ax1tkdh3s5mKXBjhzbuIvZC7dCCJWtRVFjjQFjz8X%2BS2oORSV%2F7%2FAXEN%2FM5pQ0I1fjX4BVR4vRNKqvwgok0wlwv62Ctvw%2B5s5yjpFZ%2B26CcIWS5OeRrqwb%2F7UZ%2FJAPFqV0ePd371yT3eHkavRBge1tF9CskAneJ9dWc%2F5BjPL%2FG6SffhEB%2Fi84Su052laqM2jz10q%2BcKInNG6YpsEDDd4aY0AD9rZhvZrRPat76KAGhRi%2FpC8w388BFCRwbRPTtTDIsbzGBjqkAX9JBfRtm8PrZ654tq9iM9ZIDx%2FSP3mBEQNfWz0zHhv5K5knKyjAn8Me9qJOOYiYPNlkwVoq87TvTomjmvHlGvD%2F42gzAaDVmICBpbKRqeyWZjr8O4ZcI5kLpkcNtLu2WPq8bgqZMBqgStf5c0vyvfk6%2F3XDNUdcuHPZH%2FmVLxr3HVLTcLeH4nhiOU3gAMNGQQN3n0l3YIBbdgUynqCqVeFWF2w4&X-Amz-Signature=56eb2ad585162284132981f7edbe5ffcd3091d9a478ea9dd543fdcce59e7af4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

