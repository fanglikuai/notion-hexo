---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOKUCCG7%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBeYYIFC57l5lDpXO69qvKLNoYJ8sHzJ%2BqteR6%2FlE5DBAiEAy%2F4i8lNsHNqnK3VYsG9KSuodSVB9Ww1BI3GHrehC9BMq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDDLeTRFy2SZWhDR%2BmircA4io6zABaaOxlJKr3iVa186SntoEA1RQUMVviNHCIZmVoVtwu2%2B0SRS4cRM%2FsgHJxJOKdbwP6auTYGXAm3rELODzFc1IQE3Qv4%2Bfin%2B2fv4xo08P5ABzXfHJBArgG9uf2vEvsv4pX4m473C3eca9w9pTh67sYSNhKQtvN%2FTp4gutGMS398lySY3%2F0k3MVcdLlaCWTF%2BNohBCltT%2FnXjrjLigL9oCPu6klBA5Pg2O7V9WbA1S5EAHBSeTyHQdNDwVgmewTQQEY8ewez%2Bo%2BHYN0khit751Y8j1yVuNVgPpFsdluGmVOeU4%2BOB7fLO15DpgCARTexff9AkSgIkML55kpaGTlZw4T5HmnIIKsAsSYOpY4uRl8NY9Ugh7%2B9sakaudbZpEVLsi7oJIDgdbS7MmeJ6aRQJ9Ubg0Lx1P7KDkg0LAr1%2FxsC4BRgqaFi%2FnDC8FbOheXm7PMhsQoAP4KIffIY4omFPK6TXXlR3Fcq62uRNtw3qw1cCrZX3sfKak9IB2V%2BN3jdBmwn5bkjuwCx3AQUSn0n9xaxG45ArsgmKRiRKBZNBuSvjWl6wCsoeBj7JIS22u7NlUHcJkNM2g9yVE9kz90LDM4FvCWQ3kjULr1R6m1Mrj%2BKeG%2BeSbigALMJLj6ccGOqUBWD4WIUGVr7%2BXvFCDgwYwtx2MtDlGdJNbpfPjZT7L%2B8p%2FRfKSghnMVW6nRpJaAzu3vEmdip6GaGWf%2BWi4nqLctsqduUNZcuOIN3tkCFcvr4tgsR3hoamcZaf6q0xdSnA6KCaxLnET1zmv6ouC4rdvrDrWjrZGGFCvicA4NO1JfCOPNsQ0BZldzvjy2WucoFIA80%2BKUM8i%2FDP4ar%2FomPegqSn86d2P&X-Amz-Signature=8db0c2a10700a86d57ad0970ebfdd62b566e6881a334ca3cdd5da6862c2f7a80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

