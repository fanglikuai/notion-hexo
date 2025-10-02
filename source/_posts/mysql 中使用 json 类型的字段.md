---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCPXTPB2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2Bf6ufiP03%2B%2FCTZM%2F01lW%2BtkV4a3Kezj0gBefgdlR%2FhwIgc%2FKYfZr9fuwSEx10h3o3x3TRBRvOdboU80IvzKd2d%2B0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDJSn0ZO71%2Brvlj6rPircA3P2FmGl0XHyPYZFDlxlxt5LjmTpZqSfAq0FtatX3PfT8QiOifNoLCu9Bbb4qECMMRur11ZBBtRDLcj5UJrFZ0%2Be%2BuMZzOSotwqD85uRXtIkbG3E0PVSC9LtZjDqH6Xyxxiu5b%2B2xPnfIGKsNlb3WP9VOhQKsk7hd0FrG1LXKwTCuUBOujkRJTKJ4SWekMVWlug5Wf55dLQgUKtNC%2F7nujBMfvpoNEJvIBDrTGZmsyCONtB5d3Cnj3YEft0vJTkqCwx28x%2BDwycd7ZGbUUVoRdUI8feM%2BU7xtc26nzTIrRdhdEYgSSgeqZBAXJypNUn3Eawm51dsvytGVi6V%2FJce48oiJlM5OrLtPBOoePqS6X6rc6UMSdiWvHR89PsL078enESEuoigh0aLPii1lSYP8SAQVQhigkoFEEMfnmC3YODMzHZheHzY6vsa5%2FoR4729%2FyBZ2QtRzP9fy%2FQtkRk%2BiRl1xzjK0HFqig3MFwwlw27FIoD6OnQSHw6GoouYS6y85GRgg5jF%2FpJ4C7CmUWO1DydWzNCed3MiFvy1PotOuv2OrdGh1cYxHtmsU1EZOX2VeZDG60Es2G7H0K2inW7bllkCs0V%2BS%2FClQ39d8oOku7pD7AlW%2FA12uT637EA%2BMKKc98YGOqUBKWNo2NezRcd7x%2F3l29kLk5IeBz%2B9sqgil9mdHg7lWaYblE6SuorRfirC9cnLsCUp%2FO15cGALZ2G53k4dnToQm%2FL2Y1%2FjKZ%2BoqJKuGCdqlBN3%2BiL5kyWOdPE9aoTF0gExP1nghPmz0b6cyWI6m6rt0VBY6iWn%2BWUQLTEqY%2FEH9Oddva8nPFuNiHh1v7nb%2F4SQwPdq2v%2Bvbgy%2FfNlTvGRuDH3xITlT&X-Amz-Signature=e3724bb946f5d4fde5e0f3a6e75321d6624f327d7c6560ec40695657642c562f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

