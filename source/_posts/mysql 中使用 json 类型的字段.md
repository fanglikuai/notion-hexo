---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QA4YTOS2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5n94yeCnNF6U3Bl3mMIWq9lAA3TnZH4CxCaBIFOTSQAIhAIetz4slqsS5KCi5qTmC5kwE%2BiEjLmAQFC6F9T%2B8%2B2TNKv8DCF0QABoMNjM3NDIzMTgzODA1IgytTtnZ10STAkiH9bcq3AOFVJfP4dLgJS%2FWXsLIMfMb3ucPqFZKLEfqLu%2B4rSvUuSvUNHqrRAqBQCNcVWF89kpc8TE0tObOF6BXNtr9Z25eSuO2MJINm6C0SNTfUgeBuE5P%2B%2FGrHe0yDNF9Zz20mVSS888Jc%2FdfPbUDaPScpPhz2fBrY1BKOaJe7mc71%2Bk2LxmG0IDlxS%2BaWwGu0apxEG2xxSdb53C3CA1ofkZbk%2FyrK1FciL4hT8bqnTnxPWI90A5sYM5CccdruffJOAOr2YpTrkJwGbt1CaBj0FbOCk%2FpbplZ4hyiU%2Bmsn6iWHRDUW1FMoOuPL%2FifsuZ3XMzbK9eQSj%2BQYoQx2IKoG8p8pDCk3C0T6MgMPgQqDuia2GgTRZHB1IQG%2BaG95GOyDhw5fMqQfZA450wxF4TnhY8HvORmNKNBzp6pFCuY1CaFG1sXLcl7pwcddD2SWutztHcxm6wd3my%2FVH0t9Bdr%2FBsiM8GpNxRYWF278nF2lTHg9Bc8kyjyvHNgHH5xfEueQMXv%2BB9v%2F2%2BL6VLxlHxRZU0id5xQihlJFnXgB3z3djVerjT961SWgIu4wKxMiYk1UzYS%2Fra5Cvm1hp86POYYobJniy9XzLgduvRvwLFoxdWqTccIR8SP%2FXNr%2FLIyd0bp%2BzDe29rIBjqkAVcf%2FnnbZj21RV137LJm79p6GOT%2BBL8xma6465j8KGHVzj66PKt6MafH3%2B%2BWWFro8PfBl7aNCIjBteGBwghcEaXRJVyR%2FGuZnRWXwQRtPEo3M53JXPOT1IZra4Ol6nn%2BCzV0l2t90A7wsMffh7HsQgD0qS3Pdw1X1hU3qxrOuEBaPM6xVhNI93%2BIWFoTHXpNWfdYzdxJki2k6Nwt2p%2BPQizyH8IG&X-Amz-Signature=09ee6f9ba2e34131a2270949f3b1f001214c83571308cd723237f7c25cc3d2f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

