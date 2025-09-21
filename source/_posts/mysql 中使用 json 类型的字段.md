---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HWMXX4T%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCZq56M8IyIRu6rvIWM0eMRxnn33edh79AwQrabVV5UVgIgDBKDE8SlG4XvM4plcYbAoEzoFj0DpISi0IKgjO6AzLUq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDIsUzF2Lc6uENZV2jircA87gW1qQbfxT5QedYLkmO4jB2fwBaArBiQ%2BqZA%2FDUAQpwNAiOT7H1EeinE72v643%2B0VcQXETEEV61nM9DQt4AGx9hvyXpYaYh7MlluztRDMyv0p2eD%2BaCmBUOxtVDIEyBe8uvbjWzITgL0mGv1ESks68pS0TDKpCEp3SIq1S%2F2iMLLH76lGbxZJwk4vcjTsSoPnl5T5Vd9PDQOtqfebvvpmlYRPLWjVi3ncgFGZrp%2F6j52sqkzW4ScXX%2BYug3LhBHs99nxwaML%2FSNrgTqqbSVYhSkJ3Ef0%2BJXPetMNLtmCW4Om4UDulsqVgcEp%2F%2B%2B06GU7DOnzPqjIoK%2BKjgaZQ4KJsPOafatEuM89f1aRrJFCkTbbglc4oLZQiD9tYxo2F0lh9CZIRI3GeGHn7fjX8kGhiVkMfQe4Y06K7OcRGMP7RBEtjEwR18Ifb6wzYo2UhGOu2si0VBGbSlJjajIBOLrX9F%2FUTJtcoY0ly0ynHQQyh7dDBApUFXOCRMPkl23nfYt4m7IbB1FYEIP69HKFsINQqQYjuW%2Fj3b%2BipvBPxlR%2FaNBEm0TRhp5JEQPEbZ80fvrZN%2ByIcBBGMb3uTJhfL0IkpD2AB7jyuwWkr5pg3LZRStdefVR2xn6%2BFHMVC8MKSZv8YGOqUBvV8Dzydmvgh8NUUXQI9W31v3it4WggPHmOmKsp5AVmraYEhJ7JB6ccfAldt9g4GO0Es5nRLwH9pi3pSK7Uck%2Bl57bm9T2Uz%2BFWSC20zRYsr54OZENFKnPm1fgW6gC2xsU4zII2rtSdFnmGHmheBcL4iCqvbO0XXcQSD0pP9P%2B1DSEuJEQAcuMcjKGfLEU12%2Biu0sGQEWw0LixqanjFkTZmugoLMG&X-Amz-Signature=0ae09bfeaffe230029948fac1727bff3131e84cd227c24c19539e204303bca1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

