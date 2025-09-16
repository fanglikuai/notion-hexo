---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FCQKAN%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCICP7RqtV7qj957v0qZ0GxreuEP06NLJoKHjFbh3q39NKAiEAlRnTJoVUM%2BNUzZ5XdEfQ3ozLeZYceoLBAVBPtOgJCDsqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF9bPm63QkTpKoQWJSrcA1n3UoyC2UMGdK6ABFqieVolAWHNDWsqDMEGns8%2FKdzmms1b9JkjAWbP2G1emPg8MoSUuV7K1e0qYTTObvoB8YOb3WvQ5W89FjUZJU%2FD3hJ0meondfOiQT4wuIKohDZsU8JO2BRaiK7HK48pG8%2FSSCF%2F7aHaWVWJS07YPfiRdru4Q1jUakMXv6bcg9W6KO7Bpc953V8EikBvcdDh4mJ%2FO8PVYKO1p%2FnJhIvh7r8LWsDMlSlCT4XyEg9DJeD6C5OncoMkXe8SVj97ZTRQjc5VEar4E1A1VTncjRCK6m12VF0YlmuQUiDRc0YOoqp2IrzAijGJdVchic4ppCQcHIXpXS%2FaxxxC7YF2cuJFinkekUyaNQbVXRw2J%2Fs4sAV9P5kuhHykLxeY%2F9GsxXPB3Cdoc%2BszK4uQr8UKjItZ2%2FtgaYD1%2B0O8A%2FeCKovL2lUCkZLI40wcGeSe%2Bgv84pgh%2F8MlXYF9%2BGeE3sMUDb81Usuc3Q3AGkeBOoyC2TCD0VHqA7QAUPVbEYZ8CdqpCqjfhL0%2BkawFB%2BIKyWmJ0j5xat41hUbaw4cY94VaAFdTFd9LopQLRFKw%2BCMOwZ%2FRx3hlyh9PiH28JyXlYB6POrc3WJ%2BstFH%2BpcE6c5ivGZVPBCmmMPmYpsYGOqUBliJ7CMAwCSLaKilqp248mWIfPAXBj9pISkYlphCrWuoUJKsEg01WM9ef9QPDuOWt5Mmaz63WLAVBJyupcK8CiW97Zs9lvJ4wI1O2BF%2BxfELlaYPoC6wGGSBzbQSVGmuG6aWPR2yTP2cq2P0SD%2FawiAewyCdmnXPpIEbXBLL7cnOsqczcONrQCXwLG2XGsFnnd7oAI06tL5yXZxbDLffmIDoY5%2FdT&X-Amz-Signature=795b03e2266a413d92570da310835c35bbf5bf0bca7510165498458b48b449d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

