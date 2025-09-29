---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IWAYUKE%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIGOcwqyEN9fKtLShdf%2F3cBH0sFpJqr7tTkfQc7M6ZHLcAiEA8epYZreLvCQT3dUtZcpo88hmIWFD3lRLUcn6O9WKwRQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKJmb4IXsKTAGfjx7CrcA16ch0lRumcAjIEAIcD9pGqufWcx9KkewMXtuoj82yEDOAxCE6N5kZYZdtRZFWMlOGSAUTOlndg%2FH22S8%2FQEw%2FDaU3pwsRyTykG9YIXvq3qZixQLt%2Bt5LilBG10%2Bq4pDswOytFX%2FCcaEcbasJW3YanhgRVemxrRDUwA4IyjruMWGp%2B4JtkraFLx4fnGSJFDhPCX3q%2BUjmW9RfoOwOWv5YCgDHDfCo1jjutkyuMi2jOesxlL8fleIO3b5e9uv8gzTjtx1%2FN%2FTkBIa%2BfZH%2FXK6XAa9cTRcXsTNNqasPzIlVKaIAVIUXJQKZw12aIYT1LJ9FqOsJOvc1lPfO2AJ0%2FkzyphjzAKn8VRICYXXq2mpT2L2ymyU%2B1B03F18UgT%2FrFxKB3qq3IvmKcvMPcRLrr5jsbxELTFM0qKNFdpceUCHiTYXT%2Fc0N0NrWZZYz3F9rBpEyIdSweyi%2BtrSqSuLWQ%2B5%2BB3tt75MQfrZpsk6U6WrxvSX9112uCUqIM4dWuWuxRLMLL324SGpG6bgf3wGIXZNVNIXjSRuz6JUKRPL%2FU35fzu8cwizqhD4ARVbo6sJ%2BWhMmyG1mFYEAT1nt42ynTFnEd3%2FBX%2FqknuvMngNu8lXlG%2BBDgZfPnAVXeuiTPL2ML2r58YGOqUB0SRm4r2pvu8svgVV9YDufwagpF4xMADAGtg4AYHd8jERAWBS70DcXIIhKTjs2ZH23Hwvw%2FcN3WCxkpvnXC0HgujbueFZojGCqC4DraLgEebSvFF6YOALEXSRVeIIMD3bVl8PiLVfnvBCrOfmQ5J8bZyASoRtpmE4gcHOQm%2B1rqOrDJ%2FWOKQXa%2BimtfSkm7JhnzfMO%2F7DzGGT3NLohb9lesiP17X6&X-Amz-Signature=66377f62f1b3fc106f923c7bf7c588a47de0218c2f67b6643e1309f221ae9e48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

