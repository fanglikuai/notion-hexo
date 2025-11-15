---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KCKVFZ2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXN909XXCdUfvMSFzhQgIIntae6BO%2BYtQUHzzzDcRiNQIgU%2B4wM3q768qNKyuGk8m3KdG6zNCmnUsW%2F1idysiC1v8qiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDr%2FrqdR2r%2F1QFoyPCrcA48faWxo3CShfg73cI1F4J%2FBLhKJHZCYgFKFp4awkFXLkwS7k8X1NFFpbcrDVOXuq%2BF4e1bRlMyulzP%2Fs4tWvNhh78ya1hSbYW9Im9iEjGrX6PDje3%2Fez%2BwMpoGYAlk4of3Kh5sye%2FrTwM6V0rcPSh2YnsC25XuZbRfzV23mUugI7IbKfnQnNrpXQ2XNA0%2FujdI1JwOsXvw7RX%2B%2Fh3Vx3swTluwSoJ1S9KWGp7kK55lg7w4iU8u%2BgRgtATWh9RCyqzmj0%2FHFcPUWRrkRHJ1gCJz1xBM8MAnIaSf5Fi53wpr6H2NPYddvXgBDrhm4Edc%2FiBOWQIDSlu883BAYrRlIUVDqF8OvwTd7rf9wpZvlnC1QUxnGoFkQGsb%2FOUwtYSCkiHTnFuiw4DWMkH2vWl04JLcH6JoAdxG9R9VePgEw0hcAdV0QXDsGvs9ZxzQtxu0WOAkvLgtPpfZKPJN99DtQ57wwv8SYP2KGvPz0Zc97ISgfL6rZP6r2FonU7DIECJYj9Ht2cGvcj0GzyiCcAjHceIYtXGSWo6OH%2BxAc2eb%2BgDgJnQeF6eNOFNBHC0RdlnzOqJZchdyyiR374oWrpCXGrtYsaZJS%2FInaUWdynuQDs%2BU2LhCfvU5b11Kg4fw4MMyh4sgGOqUBJ95qP3bsMShWDRot0Xyurj0WVc4eNEsFarS5h6bfgBtyJEuavlvXATFfjlEi9s0CTOJAZwawMrf7xD7kOQTWnLE6QPo8EeqXwsI7qYr0vH4xgkf5Zdo2FAn7o0VfkKQ4fSNnrWQxKdyERJ96KigS7jEI5irhCM8IkDuRJKe0tbomrEffbOs57nNaS26gCrVrMsn6CHaxXXTDk%2BWakAjQwZGExcXG&X-Amz-Signature=d3ba7827795e75f6e35dc9ed0681ee6a34d8fc903aead9948c7aa2ec30f8d3ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

