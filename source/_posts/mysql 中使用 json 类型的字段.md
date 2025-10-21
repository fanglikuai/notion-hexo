---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR22CFBI%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T150140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQDvcs2APVE47m%2FJpzivmLFwT264MKVOndWEw3R4BwpacQIhAM4BsB3J571dUp2pMmci8Rhyy9fJhaHFxcwK45aa46bwKv8DCBcQABoMNjM3NDIzMTgzODA1IgzStj0ZgeoGqbuQeC4q3AO3gJNLFmjqtkduI%2Fi7SQEnYa%2Faeid91m8AG2KVsKjCdghXif7y%2BhstAmdepNPM9FIqQaZ4DmCfCeoG1abvWBuAznxWr1jsmXhVyKuXs4bz8GZCta5cCiQL1dsab0Iy4Y6Uft%2BrVQwWpqW3Adpb7jVucIng5kye0IF%2Fj33%2FwNz8xvTIEgERb59W8L9fRjLqvOA%2BNFtFcdL7rXYZwyWoe6b8ha89qX0XkSIXDlwxw4E9SH9tDGddK%2BNttLQwHKcxPeZ68R6%2BRWbBTmvu49X3ViN469E7%2Bnd9R9NCxpdU5LYlY6Qm4Eqd9Qc42OCGeuvKrt5pKDGTCaQ9DhZIYLdK0YkKSyl%2F5DjmIrO9aDohMBVA0l3WNJTYpQLzOY4PT%2BkjhoUOCJ7jzsq6WAOFIiBgKAqD3MznIYtmBBdXfXMpr3h%2BjikKtxLXPCqMQPH0DFWnYJDsCd%2BeCxjPXwGbst1Lcjrjf3psMnwkZokqaQ9xr%2BNALSClsqj0jUbeABOJvdQt9Gdoz4HRmRE%2BPMCmrh3y%2FU2rmAErG6deXUMKSW4OUNrwbh9Xyir7UAQKCdzx5mGMGdJvB6DtE6sfSk14IC5uf5efYzm%2B7i%2F2Uz6MpefBr0wOMEsBDYOy%2FHShMZkzhTCprt7HBjqkASL8KRW1l9%2FdLPj8C8MgEokV2JVGwJ%2FHDPsoXqK3PMJAfQUhgwfbKz1tnQEoRBh1HI9hY0HhzFLLj90ZiWHNswvAGx0z4Pm8Ez3rC5UZrtKvZ1GB6HUHIN5EFS3Bdd13q%2Bqp9JNNRXcbk2dPuF7roP3E3XjETLeT1GaIl8%2FPKiUhYbjCGnZ5EHI%2BR%2Fp%2Bq0AonGJNUKb6UyFmmp6jKLC%2BDJBRJG2P&X-Amz-Signature=f0a4ae8dd35c115a4ab5c50e2a7f808595849741d64230722595771a9152ab28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

