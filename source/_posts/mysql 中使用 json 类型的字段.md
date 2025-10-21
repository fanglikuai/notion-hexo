---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PVMD6FV%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCQY6imCtuQioLiAM8S275qQMs%2FFI4Fc4F6D3YHiQQnbAIhAI8nlO9JdtneCnI7Pr4VIJDdP6KS8sKmYpl%2FMYDL6M8BKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxivMYd7uc3uRwAYw8q3APIaAhaEbui3Loi3goUfJVprd%2FHfiIqfNzDeAlSD2Lvby7e5JOmynZGiTh97CNlB8RrCb4vE1KVSK%2BcNpE00aLbgKXhBw1TVn0i9YXo8fUtD7y6V0O2W2y%2B1icgsu%2FmAzpik6RgaJNwHd8UADVfOoJqIcQgHRMdToUP2JJOTasqesnkAdfG5r8hL1jrISkHC11agafrnLaTX5DnQR0%2FZBzPLprluQLkJAnR4oUAsjpOxXnRZH4waOQBJkdFfCFJB2JPwXDnzj4CPOaijsLkm8Cj0YQFpNHYSBmnQ4nasdPTfaJMEM7ilXeMI9C3LCDMgWEltw2RNh7EKjDuWBS%2BV78gti82lsl8aIK663ZLNbHuEOIZGyXf0bOxlt6fGn9Xw3ptiGmQFX4ULAzjpOXq8BzalqiEibih6M6vI3xMSh2jSgInIrdS8g1eCrAR7wH4b%2FtDNpcnVKbW6tQS6B9K8YHSMmXfNHhTuwgy6Cr%2BFuWvABts%2BQ0bn1D8WYH38pFkYj0pAHhPPwW07rEJrfVuX4pCxm7gQXxej7NisVrg9dZBmDKVAN5AsHL65DDEoNBofAqnyjdGMTBHz8OFa2HHg3VhAAH1N7S58Tin%2FUpeNqg49paOlW%2FQgphQgRvzwjCzq9zHBjqkAbgjKDsNkqV5JTkcH11aQXJ05QMUOx6jmbI2sFkQ5%2BLwq%2BWGW2BmzKFZlBlNpSiKYnMrtzxLiWVNJEkQwXbDjX97wEUcN6EmYX5kH1WP8gpCRk%2FZI76ZyvBmgPWSiC2JYhzqfrPTO6HhZFiFs0BNa%2B1VIcGZTh6dI%2BZ2jG7Z%2FEIGPl0xXgA5HIHjkVbiV9MCuaYCvxYePQX5qEOnGyR12dErGZfA&X-Amz-Signature=76fffa8070f83c91bc8afd389afdf9985395f063a61d9796da6a00f6f732fbf4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

