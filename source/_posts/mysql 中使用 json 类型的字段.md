---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFIOGEYO%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCPDhD5PraYDJj8hZh4QQpnsyzxW6vOjSX%2FjTTYAkpiIgIhAK%2FK0iQfPu42s6%2FcnHrraSk%2F44J6z6bJXJfmCvtejZA4KogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyCH%2F%2B8vWQCWMaTeKsq3AMQR3gldFWdK9WmdVN7VD6x939wdFiWtkWgSCmVcSkweBtVQnycAG09LclvV3A2XerKCxgxRVSaOuExOgh2Dx569k6L10XyYEgMhnfkwMq5V73PV4kR09sdU5LnUjn%2BYe%2BGTZw615fxrZrT%2FxwOxrgXzbCnmAK%2BI2MDE%2FNlah0nQ4GQ0qNNoWqvf2qisDDUPMZA5pg61VhQOC18eOxJDyoIXE6nSruHIHYrBWzX1w9P9sYBFqMkD9fWc%2BgdZb6HZ7bt9O6v0KvOqrtyQ5pfHUVqg1ixsPrNKgtVE%2FwAodzZdaEXTr%2BkJayywF9zQipy15s%2FwdCv6Xz1sZr1TZh0IZmIK5GxRDch2dEuqJiQN2CpRRdnEckx6V0SSNGSOWYiAJMq%2FqeRcq7BEGk9Yr%2FSSZsJgaYonGGSozBicJ1Roi6Dq4aT%2FHqLta9Y1o9VlOALhXDhNQLUYPy0OvJETdajNKQULygFZSAgHMlFA4RmBiaW58n4CPq2GUvM%2BzMObfg9LA6Zmi3%2FnjQhz6Z2wsCBecz7MSX%2FbRSxuOUzas5bDdrCSh740FX%2BQA%2B9iPl14b6KR7%2B5xCtQWD8%2F2DojmbqWkGbDzZdS6fdwi9Mi8jNsoeP%2BQxaa%2FSvNAPHBUfSrDjDRmuLGBjqkAQ0Lid2xyU9IP%2FxOzRTS3qsBx43mJo7PNk2ocuj7gBCyP%2BWIigvOTfQPDm7aKgDpIE%2FJ2XEUXLIWtRMKl61OtJKJXHz3MrrlBQKqP8WlzL0yibDe143gtbshrF9EOthyE11LVAAM178QbPztiHupOL8isKbrMOXnuteflPpgdUtmtKxCbvf3rVxk%2FTUk6sgelYMRLuzGBNYybUuEpnACjtAPi1P%2F&X-Amz-Signature=314e5238ad8eb4d7adb51abe4ac84d9c0f04597d8dc64623e5f1dc0230f4440b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

