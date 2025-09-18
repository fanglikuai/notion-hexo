---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7ZQ2B6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDRk%2BlIpVD7APlqCrx7S00fcaRlptvn2WMan8GpCd6PhgIhAOVtzRorLR5X%2FAunavVdBKAfZmBbNO3Geo5zgMyHD8bGKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxFFxn82MyYgDqmdZMq3AOitu0wIaW1OlrsdWqj04%2Fe9RY1ed3%2BQkEA1rFrZb61lGNbrQukOXeEy%2BryNFcjEASjCJWgHqmLwxL1N3Z4H8UGxqTAsE3YMGB5kKall44k7IK0X62xR2SItV7sNfIBnY8rEAMzaUmLkXmbyq6MhJ3lSnQxrJaGjP5slF6nHADOjOtK9vSa0X81YM5hO7s6Yg2OLxc3HJsKPVoYlJ63yJZaCcYT%2BADBcA05v3jWb%2BV%2BlPQyazCtkSiN%2Fm5%2FmcAoijYlhRL8z5vQsuwx0AryhC%2F%2BgFH6yyjlrbN7Cebdavq5RF3MlJ6JeuFwdtbD6DAgITwni5edZBWZhGlZyA2PiYwqyPrhhW5Hx%2BCXbEP%2FJfDtbPzOUK9t9nJMw1uPEUPjnUfe%2B1RoK%2FovcjLSv0uxBP%2FVlefFoAVmZZGdIekMSSVRu%2FziYIye%2FBhJnN8kAcra5PDHF43F5As5WBYaRzei%2FiIf5nAqSId0WAgsAVtxkaI7hgJAZqr5XtiGtgIjJ2quSiuUZILhTlVvzw9xYXRuW5rF2WwuypOjOMhiUNgrgjbHTyb3DaO8LdlIFZHeIVfbtbt6fW0f4p3maVZ24j3U%2BC0sYOYADM4sHSXpTLzSnI1%2BBtyuBcSbYaQDEwgDvjDkuq7GBjqkAR0PUaLRg1PVQFBFkM3McsFVbh9OldnGZoY4le4KhisjjTLG%2F9hhW525nqvkzX5FmtU64HDnlR39TD7q%2FxU%2BUfbETSF%2FBJLNV%2BqN1RF7%2B5POGNpiNogbrqQPdTOnEUJ4ezYvOvECGChwGugGcIzQu6exwG59Gbk19RwC8vZOljOaPAfe1SF4PTDW9LEvNAnwWt2K%2F8vRk05ZrscqeR23l%2Fwo%2B%2FDU&X-Amz-Signature=8756e78f0f1704bd857379462a32e1ff5871df835dccf346b4edacfe093dd8a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

