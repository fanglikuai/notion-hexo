---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VMWVIAT%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T000037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJIMEYCIQDMmSTrLTPg7XR6NG3NEIPWTbX5YM7RXS7zMMp9%2BVy1XgIhANw3eUTV9%2BGHVt%2FzqiC0fjCvErXYlWJ4h7c1oC0fhEicKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxkFcudclag1FBfSw4q3AMOT6LUrEl%2BbuMOrPhQ0x0jNmAher0cgdwedY9v78Ly3a3wseSeQFHt1Y%2F20yGNPG19jC32xe2143%2B%2F7UptqyO0sXqBkcSRkKy6Wh%2FHLfedEvByptt5HJeooxpMcydvpUQB8C%2BVfLkRh%2F1DGqhvgP4mV%2FJc9yuB20dngm5iF7FwK7jb%2B9%2B30OIitBm4L0XJJEfTtl2Yrd2smfTwPiziM7t2qT5cWruwzT8Pfe0uoC3z1CEwhkZyFcih8JPwdSejDPQc%2B5aWAjKRXSvD2rk8Z8qeczZCOAxfkFOheUw5eXonp%2FCrY1O1VqaQo5L%2BnNC7wHS29KRBx6tK3j3Z02RKi%2FFK3LvYsNF5hypRG1QhMZE4KKj3iVlV8GT%2Fyw3j0es67DeB1Tknafr9A1lcsUOfQEMIpbdc4%2BHlNGQoaCU%2FsFO94JMdESn98xAvvqk2pGStCp0BI568T4vuvHi%2FnSs9rkfQbxEPXDIDDlI8B8Z0Csg76RRWNq55qhv3HMMW2qfyzh1o2QB0rCCuPwuf7%2BGvBpOBfuuIY1PMCPe%2BVHwikzmP3mdavYgVJTruZr8zDg5Ye5cDds5DkGI9ZT5UA0F8y6t1AUzj%2F7AfSPSVmf5FVOP56BCnFjXYxCMibiO0CzDp%2FbnIBjqkAa5g4P654ratxWS5cgWy75pfNxhtnDW8ZUc0IFz8GyxW6k1O8FxLQggig9TxZ%2F9ljpt7GcnWTnmtoQbz4WcUshXfkLXieH1rtfqk42J0piTMZZwZteHJH7YGhoB%2Fbz6hrnq0l6sMuoZYnFAq34RZT%2Fgg3QEW2mzxQGfQ63ink72EdICx%2B8qVyT1YdXFJUNNG5Tsta4YTjIt%2FgAQh3BXhwdVje%2BxX&X-Amz-Signature=dff5db76e3b881277f5eca5855a5cd415baef5359b3ca31ab70c45eb6b99edb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

