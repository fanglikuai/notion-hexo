---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLPWQ2X%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlkcWJ9cFD3smsTVwZMvIRFs7oJKJ55IbEYYf5ikP5jAiEA77nMYobYPDsan7u1aj8t%2FVDrFNLoctmVgA4HWjwfkZsq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDARZNjKMKSEYmUvEZCrcA4Rl5Z4bI0v2iTJqc2xjD%2FPCVO%2F%2Fctbv%2Bi7V3jk8%2FeHbcQ2KV6uSYsZwsxxU2nUV0KxEMxMq6zw4ftIsC7b%2BMmrYxzmgeoAGIscSSMdbJ55H2JhQyaIVkLzTCYbW%2F%2F3QXa90ooE6l3XqN1QEwSxECtoM7E%2BmGCKEH1is1Aq42Z1YGqNC%2FY3U%2BznrqEPT1b1k%2BuOFqErlSc7Efbj3Tf10f%2BHRIorM0FwD6XiAn2VhscXJfcYF7ZabykQ0ifJ0OpEmLu89gc0UiGm5%2BIH%2BDaCC0GuUuhZHS1zAwh1EgJyboPd2TK5F2P2gLPQmdtecP8ESPudnM%2FpFWyLGd%2BRNlR8cQ6RstST%2BZTk%2BpajPEQ4w7%2F%2B5Mn9FTt04dV6jKV2fBpFvSO28XmJJ9HjlwmqZqMCHKVcUrG09dBbiB7DS%2F%2FF0O%2BtWDMQDjIT9MnwV1DlZ0xO3XcA0gDNFZfF2RkBtgnW0m3eru00LUvChy3x%2FZMpLbi7qWs3yU%2FOzEGkVlIWZOHxnmENYKoG4h5xcDRXPBOULl8mI60uJvSNXNJi20R%2F8QNOcLUvYtACVj1uD5Y%2F9k3EQTRUzrKL3PJ2e8REHsFYVYxwOAyYadyGKN4Un%2FhXbv87NJvbP0lPa9tPvOWDKMNupv8cGOqUBfGwekJOB6CbiOPp0pzSDdEHl1VzRHeL4MWtW5xk4m%2B7PmXdchuXT15C%2FLpbp7viQtzDJmHvdQVY1per%2FuVmYJL%2F5oPpJlxx%2B%2BEV8UE2x59ngwkIrsenjgpevhT2hkTqyDNlWrWStSFy4A1vpiCIUkRMmuHbW5UaNE63sOqViziwpfiOviU8xHNbVPvLGG3dF0WB2w90LhtS3%2BIaWDFxU4AVMKOSy&X-Amz-Signature=5e55238fa7d2a73372aafb9cf16a6756adb48022a367079f97351f852b8e7e0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

