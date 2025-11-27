---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ED5NKWY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCJxx1quKcTDqt9YoDjyQ4UOwwMVSZvMkiJmsn2TfGNwgIhAOVJJOQ3tmKSlCd3Kvomq5GUKWNdckzZxoFDM2QX7ietKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyxk1gpG7%2FbvhNJ99Uq3ANw9pPKwjrb4PSnnLdg20ScPv5Pk16ro14msSfSCVsbRph3EfOeiHyBINONCYMu%2FvJSBrhtwUJXDD%2B5eBxQrMPquWVTLVOHmxwQb7EeMB2%2B1e15LwWYVYk9oTobp15%2B5LB0fSE1zSXL%2FJo3Vxs%2FwsxTBdZ50dK2QA8OlEcdXLy110WjVN3t12RWVCGsdQgedt0VyDGC4HAm54dCgHux4r4HrgDh0iRdKM%2FRhnSkhq0xVNQGLBDCvnX5zDYBaRDLkxs6v7J35FHvHl3b60bz5RAut1u5WL9rOCKaT2%2BD8riAMjZYDZ3AcOg%2Behmv6aPoRnd7fXp0IJNJObVwKgKwLgfYB4RQYHGdTHkHhamqkaq9zipcBFC0ZlK0QpG%2FYfg%2BEM%2BlxSE7jPlelZfW10Mz9hQ3SC8LkoP%2FuFPB3Ho5joKMueWXyrXMncG2MKq8ABjlqmbYdVbYtdwsIIP3yiMYDGgArKHlMtKJjrvcTVaggkURdTLDyohB8SnpkCqkTHMQinVBZR9DaWe%2F9wZErLaNerEGuiO8%2B0R3pDkpzN3clga5Y3hoyJIM4lwIiA9lMnrmkw6zFF%2FCWhWS1mANvFsAWXqRBPxSnj2YPGGh8L75JtdARCP6lkbrS3L7Jlht1zCd15%2FJBjqkAdDeEpMT93JaPszEiG%2BKNki92K4%2B5nT716kACxLGaiYT3tFb0Zr0edQmWWT7dbkt2lSm0F9UCUA%2BnGJsgI%2BHUrt1B32Z9RJQ1qVWQeRiD3nPtBfKlJdT6Wf4EdqVDP92F3ZOFkWW45Luyl01ewyFSaQa2X6o%2F5ZLNbcSeaIXmrIbMymWnP8hfDXxIs6E4h%2B6kFW8%2Ffo3VNEUNc2oobdSbE4Vj2Rr&X-Amz-Signature=c32281a184d4aa255a49b8d4d9dea1fe0ae47d46a2bd0c82fdfb73c958c83548&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

