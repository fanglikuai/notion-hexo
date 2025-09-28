---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDJASEAF%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIFYobWmFcLSjv%2B5m42HGoOjG1yeDhN%2FXMwXAUtmM7q7dAiBI19snlBthXMiux5GE6KntZ84MlLikeyc8hGKJar9VhSqIBAi7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQCAOpQrkiN91PtAvKtwDzSvVA%2Bd77yCjU0xPCeN03FMLxTh44Grll1AaClnMu41sAUWWOXkGt7oFVyTMPFaR6JDVJr%2BURQZdJ5h5t4wKGwmJqtwr1f4KBX627RONg8WBdu3f%2FtlLRMKQtaSMVRKaXYqGu36ddiWOg%2BRj1wu110zqVIgMBn8XDjXO6M50an5ScBvFid7L8YBU3ehBZN5nEoWk82Q7OC39ZW8QfctuEe8q66Uewi2dC1MWRgjVTyZm7E9DCS9f6%2FA0g6j1wxXxSlr%2Ft78OlRBVWXeqW502GlmbuZWYIQGig45iIg4oS0dfy%2Bddd1IlyGQhjlbq7inh3YaEwiK9FmV7Vr6eT93A6MLg3TaHMSMrJV7cC3EXr1uoH6EY%2FkpwcCnLUnvOF3JYUaEBZx64lQlaoc%2BO%2BsX3Odzh0qSOY6gl2716iLwG4O8xhrgWDvlxGZCbGFAV8o%2BBzL0rztNqtWPJt4fmjCpC%2FNs3a9m4gV38OcQ3ad005vPADm2V2p0Vk4Q4A5Ev8IXLXkNRVztgqArL79VwNzqkldUFm6C4GqsgsjnrgTxfWcLoRsNegkcWEjI3wGls8Dp4AThDI5Alj9yG1eBXTODYzI11y8RcZzi3JddPsPxSrwX3roRAKHxV9b2UDCMww47kxgY6pgEjAi3kniMHYEHVc4%2BnLz28isSux9pWH8LwHJlVjnjPjlwwsXRNPBe32rbsSezPub75TyprtN3fBIrCGnO%2FyXY26ag2nYfHpQ4Ti9gM%2BfXhFFmMkVRLF2kXDxzsrlgieCFtW9cb7%2BOZGZvWIouFcPDWH7QNbYp34NN19Cl%2FlkfU4tDIn5Dzt1oUEHSQpT4gTghHWcVd5fuRo3%2FU2goTVh5lwDDLHLx1&X-Amz-Signature=02de8044472697c96d7b4e63b169ef0ae17f485d6c655a02f6c09b157ff86113&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

