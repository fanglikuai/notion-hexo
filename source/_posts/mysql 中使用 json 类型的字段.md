---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFZ4SO%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIF4ULlqV7IQDL2Lm49nSBtX%2Bqr%2BuLMM%2BTtifucYu%2FmWOAiEAvQ0AK4Al6zZdLD13vKiRTtVfJRAy3Cr37cglOtUxZp0q%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDIR7fp8K6787A5NXCSrcA09GpCNSvSCyjJh3WLqIYy8QqffZelvcTR5q62SMHpQFj0lvuXj4CbrvwBNSXwC7Stvzo3PwkdzANsL34XjaCLiMHQL6YncaJ0ca1w4asHYA5kBugwe3BI7UX67XB3IaoSXd8zZWRnCs4nLuKP%2FKXb5BG23PL4cv4kJmoyCrhIXUsGBnWNqN7MFeM1ivMvyUY79PQu7eR6yZGlLjC%2F%2B2cerO1SQ9xDyJY2q0HyqDjnR2Cx8JpROs6LxQL4P%2FApd5qj3htx5DVk35ZQc9gz9rsN0YKbEkphvoiqk72pEzKSWJvD%2BqN9wIBkjsSY1n0n4pnhZQKC6kngtz%2BeW4C%2BEWt6%2FXOQCqAlAXgT5gyd8tl2VdZqEMPURYngj9rcsnr6sxpXrHopx9kKfe9uin7ZgMYr6b6oLwDkZTtPnB%2Bg%2FJ%2BhyZvFy1LvDFRXaAzcROnWhTcYZ6p4MXrY45wJdvPu0fuKqmDWy556Em5cVZesCmv4dnhkMdP6baCq6n%2BGl322mKDrizDslMp4Wu642ahtReaOoXZrlAufPTicmpUTQhmEPz9SCm99gGLQo9wTaZ%2BfstoQoHJqAFsQEP75SOPU62ZiLNhKZtSNegGJPW%2FPBB7nRdCBNSe2PzfvhPijG1MMS71MgGOqUBOj4vCsjQ%2F8UbTVLHqFsVDWCa2ykjV5ZGaX%2Fz4NdtdsOEOVegxqdygk3LEfx3DdGpylk9ZF97Xyn%2FGSzzNDds0g5iaR4IBmBJ9YW1bgwTmS6ylIugQJ%2ByknbzSiXFuuI5yKHdK8pfbQTh2usZZUUUzL15sO0%2FsR%2BevslUcYoGvjky3UoWuWbLoXncsC5%2B5GcXGcWm6ugy2QNbmd7HBsor6LM12ebl&X-Amz-Signature=c958d1080de1cbb137403ac03019fb7b11e5781e998a3d7b9e7845050c538a49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

