---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ZGQZSL7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T160230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCICw1T36siA8K4lt3BJcykH6pQIoDcm%2FUcPfwpUDEwL3IAiEAiymgYdy6yw4KGoOcc%2FRjdDCoyK%2BKevVu4SiBoUk1N9wq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDMOo1M76eHi00Kw2xSrcAx035vNtg0wOFwoVn2vXgyIYPfG1U7HSnUhyyIVYFaMsTK%2BeVC2dZqnDtG3WHbLos6rgkQcqw1u4suyZ5QVKhAattGFRriRMsY6bLl9KqbdUumZnGWlZpYQERqYzMerjeP48mt7j07B7tPBU0WNsJZWBwNG7PUhxxFJ%2FsGI05y3lt5xP2WccyiBGOHMDm6aoKkdqexC5yLtnnCP0Z%2FDVdMQKBcCpiKXIL62pnHiKSfpeoeZErFQwKfOpcZwXZ8sWqCcGdYONPkjhUB9c%2FaZhjmfcRlctnAUh%2BMHfsbol%2BfvQCOyUz3iJw6jFEa%2BQgA8n2y4wpOkbVIQx6vzfOvL47v4wQZm9MdZyRsw99809N1FO74Ig%2FsCF%2F1Q1C%2BT336oJx%2BxT7dCQ1IPrLFsferzQN3NLR%2FcpKposjff%2F5FKp47tmGyq7RKyvLCwMQb5dyqV7WQAIz7Pe0%2FPH%2FI0q0TFYGEyxHurYkeYWgUeRJlZzC2s9pMErg6CtWTaOe%2B2ZyAnYXqP8ielLy6C0oC%2FSZS2aUt6MxNLuT2kwCzdg9cXs5Cc2e2aIdHzNbjJwjihYHYkjQSer3RNKHtHG5Fn8K90KxQqaaOfRMVjQfzx9wtM6yhmqrMcsZ7eBqXJSKcrfMJi6k8gGOqUB9xVOebYY4MN7sRXly5fPHaUuBk4NX2yfRihDMMH7J236ogfttE87RDf7AwF2DLWWEDmhCEVseH7I2Vv86QSVbRHixV%2FghestT0LPT7YsKHvr90Ag%2BwIxVMaa0k%2FdZXMGGc7sUWN%2FLwM8p0778Eu0V3L4q7nbLLuxWdnIqAzC%2FsZNqTSNpZgqLVmowY3I2LoGLVz%2FDl1hZvt5IcVOhzD9ceMycsKm&X-Amz-Signature=ccc41baafb7ebb033951408a36ed6556f666ae2a53df31d9804142c1d1eb8a75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

