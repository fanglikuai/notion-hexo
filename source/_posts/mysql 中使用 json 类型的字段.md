---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TZLKEHNP%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEqeLDzorVmaXEyIWhDbWPcnRUDl9zlAXQ5hsky9rqi6AiAUPvfrjrSOAj%2FJ7KctaOkFIw0Fo0gaH6oUM2RKH%2Fa7XSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXVkGRrT5R%2Bygr4NUKtwDO%2BoRoqv7TcV8R2Y8f4LZI6yOMdjpVkPJ4Lp3atREszBc01ee4wX9%2B5oKC1VGqVSdmufN0b2TaGYCveWFizjkztTsVftP%2BYbLKWxfnypAtdAvbcOl5934e5x%2FzxshSaK03NxyCVRjz4EsY8S7Ve7uVUKCU3yU7X26RthFkkMY4FWJRQ7ST%2FEaOSpYBUG6S1RhxezB9Ro2R64PNdBkFlqy0ZrAn6t1vZ01jWtPKpekh6Koxeq8cN8jOW%2B8gdXjsZZJhZzoAUnvqU1uSVo5erW1Ts7aHp9X8wffHcmLDUm5yb2xZEImbt5OMh1%2FSaNnR3rk5M8MMhHZrVzgC%2Fl6DrbrcTacGOToJ1WYzmW12lVCz2wJiWYSRu99Zb%2BkJQNxIBV3ug%2BsU8b0NIPWdHTdGK%2Bxk2MIOEnCm%2Bj3ytedk69zJV0lmkZ2aPGv4iejrQx8cRxvXiwVUtanOpwXyn%2B6erRhk82wSabIKmF2nsRpjxiFpGC2d5He3Bk9aH0c%2Fya6vuC%2BSDctwjZKc26zCbaFvUnxgmjXmrvl%2BdVolR5kfDx2aYC%2Fdl7DqgWRyPhIJFm3DGPgCi4XWDd0uPBGefTz90sc4Ls65BJC2BXGBbbLa0QP3MiKL08jI3c6UDkkNOsw7d%2B9yAY6pgHtG%2BKwzthI3pd8i9oBm3mCIosyUqbhXc1DXm0cpbZhC18B3AJ73AF8mqTVk6HjsGpHeAug4nb5W2VzbiIAvvT7vysMYPkExBvByhsSmJDltKT3X5TLkoHVMQUF8tOK8ydtdU%2FSQvRcgo2zY0CC93KY4SBan%2Fk4CHx6fxBNJcLQML9FbhKT581iM2u7vxFklb84cCNncD95Li%2BBXJaC7UulFi4ixuL8&X-Amz-Signature=c1242f3fdf4168a4ba817d66a2846168386b8c13c7850624a46556beddee1f65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

