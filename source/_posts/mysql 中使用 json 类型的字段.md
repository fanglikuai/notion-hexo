---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQ7TZF2D%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIGaZ4eMYpEuA2tYT5vE6%2FqvePqIJwYMq9NHTTWz%2BP7sqAiEA3RVXVgAFejrmJ4SARc85nHGbd%2Fkdm%2BIs%2FyOOL3zN6uQqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9zKrmTM31xmW6oMCrcA0SEW6ckyo3luzHZOmylXSW5QT%2B1F6K0RHMVrXEz314ISBtawNtuF6auipnvUnODfTmIKtg4MwNC6jo3E99iSyLR88zZVdhpt86BiEnAlV4rdXgRniCHqnE8bmX1JlL53RA49i1QhOqiXe5TmxU%2BBShNzGfdhoAlJnDVjBOnI5f09FmadUq50YYzn%2Buuo5kFV1t0Ea1FMHgpDK%2BRsj%2Fv1UXXR8CejTMGrsXF2ab%2B11lOjoL5fLwjuK9AQo0VKPwbhW0LJNpN3sub27hWsAGS7RYuCDTg7c%2FxY3rURGEZFOceitJWROH4LHLRFGnMc%2BzSGtY%2F9Ka6iRCC%2FNYmV4pr2qOGIGpal31XpcFVkrqty0OaCCdvUhwhD1XePGAAhzq0s2H1zT3h%2Fgp%2BXCEg4iaYWhuqi%2B%2FUz7I8o%2BM3Qr4Mk4o0XWcTwyYveIiHctRAV0L9CN2y1Ok0xVD4DHj3WSz52KF0Lj4ouj%2BI9vYnS8FLEFBZcxD22%2FYxv4c1wZIrm8j51HB%2FuYvPov8HlGqd5n8KTNXLpBdihWGmD2XrIwz%2Fy2Reu3ifBgR2WTcjicB4yYSluP3gvkzWJY%2Bd03liqqgIDdBeJEv2vcqCPwdzkAIWDOwXwcph2vWuoMRNSOYnMJvIz8cGOqUBar6grWsH8Dl3deEIV7qIkSmlznWLkbk7EZ9m1m65l9QsQ7a5N2HKrMaFzj6jDf1OpNdCA69yvpV0fmrmHqBctKG1JHrf7F5w1YmyyKjxPYxXXyqmZXN6dsoFeFS%2FZ83nFL3HigNomEeNjJHRv2TqKt42md4B9%2BXAaZkzvZX4fbMSreL7tQP2TiN0s4Uw8jFYZiUNTdR%2BaD1BDX%2FU4YwUcupL6wTi&X-Amz-Signature=59fcf278b4368ff55c443af88688af8d571bb8d89c216aa775bbb2e0de19ad8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

