---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIIC5NXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCDDcOGDLUiViNC1%2BLr1GMCUTxWbN2sTFxx9fI5Dat3WwIhAI8SHORk83VSXnK0WKpMTU%2FfFbiPpEgL0V%2FRytok0%2BGaKv8DCFQQABoMNjM3NDIzMTgzODA1IgwVddjomAn91FO8eXEq3AONk3S1Hf3rPyoVFKQddcvPdegm66UySniu7JwByPR5XNhUc2m6CsaAC%2FH7upz0yZspMjF1cQjvfma%2BM1I5NptHdL7fE5LPUzgj84Fo6BEqt1wPn%2FeR4B451463Sis43ESQhIKxYMIgSAhwYkwPa3CS0fZ7SWpdv%2Fm9dtDE%2BQEq82DqfqXw7vGd%2BiZJYMCZyB5G2mwe0%2FUgTn9qBaE3DwMGyEgeY0kYVHpAeNf4HOS5TNbzaY1LA67wYDJ7fpgRVmUXD5DRObJDdTSgrba4ksPNO3XfEgsHSte9mLPpznvvAivP%2BvZuCKQ7leiVhSZWa6XChly5MEimq%2BTmOTo7XV8pyrMy9gIUrYfORSyf12QcJJpgP64CBB5%2BWEeJFQKmcGi%2B5B0evThjeRQDV5kuE9BZ7l0KHoqj%2B7VryhqBObGUzg2qK1vm5HW%2FI8ALxZxEc1bMMYUxaH%2FiCd4ZXG14AjDwTZXE5xiA4euDIGXXZ83TQLYpSDeFSLFXQiORXazDDHZ8%2FVOFx2mVs0J1QCCb9Mx%2FM7pYr8qCp4OX9JrmGydHAGAxWZ01WnssMu9Ul27o%2BFUb1Yeb2qwlJS2ZRWGmn%2BUCuSKcCS%2BL2qzTSK1TM4azNsYe8WVr%2BbT%2FYRf0oTCT0NjIBjqkAVll3gtRuz676hxqNMZo1s1osL4MpCKEw0DXzKeH%2ByDnxMXVE4%2B2RXvgmjemdBWKfsYj2JdwC9uJ1do30F3kFAkphxu1D2PW7Rg9pabmFag2c8ceAd7wkDS1VVIXsk5eJibc%2FIvOUFOESnLFVuzQ3vFJMDamwKZDr%2FQzMwTzR97NmEZBnStPzTptHmMACsbVcrdC82kQ2C2%2BaR%2Fv9Lm9vshcnzvg&X-Amz-Signature=6e391ed9cbe57d4fc12e3d46b2d4cf89e7ba6c8b43e64b3ec25381c7c3086d9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

