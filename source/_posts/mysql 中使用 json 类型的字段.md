---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356C2PNT%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T140038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKIyldNZBg1aiFgQOjacE%2FBNi7k5sZoqp55SNHfVGI%2FgIhAInSvSklLNLketZEoXYVeyFsywUDYDLO0raBjQBV1nPuKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxnmLofKHmFsTh8Hr4q3APGjlDGdjDZPsChpa16A4iRE5Q6x4Lr%2BKzDJS%2BbETFP5yjceSqXfXhticG7VBlgN%2B%2B4h3q%2Fk1ERb6fip7aUei0zX7Fl4B6b%2Bzuhi6SeWBcRQzY65rGlfzL9z8IccYi%2F1NUmdR%2BVPGBo%2FBZJYzItJ1mX923y7kcTq7Kvg%2Bz6l%2BzHzk81IYqpgWBP%2B9Z5%2F0HVZvJayRex4yw35yyH2FgEN4RFoMRmUGLO3L8gYe50L0xaWlVxb1AWURio%2FrsJ00UmfSHEZ23L8auAMyAWpC0ioZEQsWXjvdvSSGY1m0awdc5yGTb1riVjG1t7EPgXcKuNaFgiUIVZEFSRirmtHuVdZM04qtJ0MhZ3Q2ci3yOioB9ROSW9ZButaUl9rxsH8Ai4g02xver9wDrWVAIE9YUHR%2FTzXyz55NzMyoce45BXgJ7zqCXxNgfXJ3xezrjqIJOhHkBPryGpF73cwM%2Be9X5bcz%2Fj7bepq3re0yCkJOEDje9aCHMeRiGYAZUMSCw1uN5kx69iPZqAlObfm8Y7rOwuUC4FJYatMrr93oLGUTWQZ5zQqahjTlGSvc20j0qTdc3%2BpyENAuRePifRs9p%2FOK7xQbSUEhCHw0HlyPKE1gj0W6C5IG2bYZbTuokNzDH8TDC56cPHBjqkAYW5OKWO8ltlFru7UomSuoVuTaiRdLs7clunjHBROIQd5eELOs67HTnqgRPAiJ0IN2UOIWGR8TgW6lkeMggLBSXxRSDPnMRY%2F1L5hesXQElb%2BLf%2FiIqMO2SRQ427P89ZXy%2FU48xdCSZLTUz1f2EMtRviOmggYJ635%2FYzPdLjY5hNUHRDSshbhVi3zMAivA4Xyy45KfMUeAORP40QocW%2FgUw8gXno&X-Amz-Signature=768823ef6cfd74d12a1760f96d4e523abde56c84987574d0d020ef38b1d3a5a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

