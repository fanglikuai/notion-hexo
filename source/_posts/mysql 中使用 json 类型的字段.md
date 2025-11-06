---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTRZDVZ5%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBTL7FBuLDf3bcGMKvyGM2otF1UeVGR%2BqWypHFfBPfBbAiEAnTDD%2FfVjfGh2odY4MZzr%2FWQ5VVXb1Fe1IDp3V8fdTcwqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOAQReil%2B%2FMkKJtKJyrcA3WjuSVYV5LZdnp9GngiNfV3fxlq6A%2FGfNBRtpzBqO0%2FxO5icMmqq59qtNCZaesUFfdJJsdcbQEFoEsAKXasPWQaU50tK9SBSCfoqZupjaoZLukUK8vwGwX%2BiAXKlbl39b8bJ8mY6VGay%2BeUCO979X7gb5vKbTV9bO6b1lmWxss9P%2BFyojUPsqTiCFJs5RNsQ5eDMRJcJfM2tlljshls92vpBGXhjOyVAf69mD2NtSTGj70IziSSZdagQtAGOLEYcOPgFKYvb3kl7yLqo8FUse2%2Bu8itGdQszv0hIAHERx3w9L4TtcCnSu2DMBCcKCrlFFh9KhsPu%2FzOW4pD3%2F%2F5haYJdRo2vUakaaSH0bbEn1YoomOWMmvPkGZDheileZlls%2FMluTbxkPC%2FCKMi7DUXlOi%2FbEY8TVCouKC8UXJduoeULXrjFIZzSnz%2FNRL79VHMzN8nhxtRxe1C2HmNLME%2FVpYUj%2B0eS1f9z8qd3Ncr9t1rMPgJhwLbq7SLYWGbHMX4Bxx7yF6X8U00ejK%2FGjtp%2FJKFEMZsKepQM1Dz6zbGK3mHYd1RI7UK8QhQPTAVZ386o%2BNiC2PwQyQni%2B9X%2F6oEZ7cAK599aPIKWpfEO2I5oRGCqz3yIbRvTHHZbt8aMJLDscgGOqUBQce9vcsu7V8qBCa8J5s0yDd7tdBsK8HKvWaZwJBEpaMT2H1176XpER5DA4%2BqJpsxeuSkv9%2FQ7wcK%2BOD3t1PU7omp1eLQbIGmUfdz2FrEdjLUyDlUIM65hVf3dZxpx%2FBGer5tpx8lWHUICCpCxpDj0xGZKW%2FbV2pj2nD5rgHM2tQOcXZxtw9ZxzYAxc5CsA5L1nlePjQg6QIZNKyBriVOzxfea3Uj&X-Amz-Signature=36b1d908bec1304a54672eb66479b83332bef6812a143410126c5cfa652175c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

