---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVO2XU6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCIDEuLqvVersfKSjWj1vOR%2BZgaxiq2YglH3jc5AWWb2EkAiAbjPEdpPQMWntw6HkZwlflP9UXCBatPgwK1V%2FDIuEf8Cr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIM6r8xUOEW1Cn28ZLrKtwD9gBp6P41DXUZl0xb%2FMytkP5vCtB%2B0RtIVWs0GhtgGSqiRrdjphQhe%2FkIxgJDM5M7POMOJgOMN61qps5xN9Nr5dLqkQ50zvroN44U5eZk9P8fAOP8GsC2sc78onr99c%2B7rPat9ww2uU4NBYu3FFDgFoGJoq2yprukckvqsyD7YHmxvJdaK5HNmVDDLDZ9xLV4%2BVxjf5Avk4NV9q647RhYtuNjvd2XQfIftkSAMUJsCWjKAIkCGyTrbXfpRLuRWqc4A9J1YooJ%2BSVD24Qex4Q1OuCPITkmig9gRVfaYLL68ZHyo9RCLVIggDRcce6VjOd8GBXbPXGcy0wA7%2FEBOLEDWjcshYArr79ImaBfydDnEMZlL0X7Ncr5gRgRZWHa740iFoYPpTPYfXuA1cXbQ2VXE31zW2bcip2zNFsmU5CUyqll%2FCt1MdX1NM6sHu4qRhTnG755FDODQP0BGKBGAvI1d644xwB6BiwIox9gpHvd7NKAxPzfa8ukBDoZtD3dG3fhF87qwe9ih8RDRqtMp0CCVmHACx4jrzoKAHQljClUmDvUb1qtmh2yFoXgaQLi8GBvEVY4RXOJyzbEeb2zcjTHpTcQMPSxA92Vp%2BDhRaYQ9MwKNZrUqPreAjN%2BmbMw67bMyAY6pgFQInW3U%2FdyIPvzwnaijOSaGzofOALIIo5fB9CpNOCVjL05sns0nJgX%2FpWVMWDvlsZB%2FQZ%2Bto1bxCnhUPv5T4lqSTm7ebc2XBTaEVP22AvOHpahvjm93rLuHP0Bvi7vQ6zZ8kZRPN1ixXiKjyzHVLnfPNSM9Vtxh89Wx2ueMSwLnor6E%2BLrq1M6ZfJSD3C%2BnkGM5k9f7QUVRzmH57WN0Rgg0IiWqyyD&X-Amz-Signature=5599dccbde03358b9fe0f9fdf6e04a40999718aac9e11ec1d6b3cca9c3b1698c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

