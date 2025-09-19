---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6WBGXAB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T030108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQCyj32h%2BacCdU2JLLnnqf40RTIh%2FPIuJ%2BkNd%2Br3cYn6UAIgT2KQbcegNmHbUtLTx1Ocb214tRcZuag51DDr3e%2B4tlcqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIr%2BfmYuyU1wZako1ircA2152UWJn%2FUmrIunjm6er2A%2Bq0L4kJb6EwKDMLeeDvVyy4l301qhnF0zzTh5AoA59pu9%2Fg52Jbi83YEVAoFODsjJ3Fo%2FhqgCsKufSeux46fRrPfshiJz1TI%2BlrvULDKbxtX4oCzb7tr9490Rwt3Ll%2FD%2FmLiyJjCdsL6rcsoFac28xLESYBb%2FxHYAM0eP1hlceQMpPqoI7VpW2R5Sb%2FwLP4BGGS5trFjValy3OiMh8hW%2BnpIFLfyFRkaT2QcnbC9Eck%2BcjnanEA1GPX%2F8GUwZfzrxf3SKG3CUMMOIrnmr4bDL4wok3EJFExcOELri3NfDANkfGNOAbAtD9Xfljs5jM2DptMJTSqzlLo8SAcwmtt1Hcl487VEFR8stZU2WJP9iIzbqqaaj3fGAWf8cDiIiUD%2BsJdBOO4RNLxo4H239Qsg4Q6lBRq99aCP5KFehsWSAmUdJduoBczUTx8pKsF%2FQgQenOsYiaUhICNyf3FYw8%2FQsr5t29e39Lk7rfNzJUFVqx8fxd8obRkV4Jn%2Ba2k%2F%2FUa50%2FOaqBj0qlVuJ6xbEQR%2B8EJWEZjrQyRmJMGtPBnUl6TW6QGCJFh8Aegv7MqSXjMznsqAjoJNiih1NR2%2FyNx8EOhIUmqKAd01t0qUdMIWgssYGOqUBtMBsxygJpwCbsiIVKh%2BUt6rRbS%2FF4Dl7nMO1WIj%2BGNca%2Filb2gaL0alFWveNUOa0ugAwxuHhV817QInNw6sMEVNvGrqnDkp1WL3gWMia1w%2FqhSdYd8QJeuLPlJysk5z4ROGYCCd7tbkcdJF%2BRnJnLI%2BnP73jxnqjS%2Fe4dN02CN8K%2BJQyhKh6blV8uIibo1enRVTfyhgkDTLUTVRrWGz0nEBc0StR&X-Amz-Signature=0ce7b8db107b468488d55fda773c2d34be9e1844cccada1415b2f5e5fb4c31d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

