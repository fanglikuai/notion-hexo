---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQNUUFI7%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQCj6r1FSU3zgamnby%2F2wfzt6poLRw9dpG%2FA%2BeLQEneB7wIhAIqOWbURd0FsL6cwV3HOh2OaQQNDUwdTFYbIDkv1marnKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3biIusUuscq0yt74q3AMW%2F%2F25RuVZyKGaoilDtCmpoz5ODe%2FdhVprrhyid71h4PkOddDykHQaaPrEQCDxoS7Z9ib0En5g1DuBscFzr7%2BJu7pflDvHA5Z9yK5m6kg7t3%2FdAXNK12xEo0abZbCOs0eyJJJWZsCgEGdg2fP%2B%2B0ALqcbyqTvcLQePDH%2BWaBJrCXrwZXIPznYSmZvItI8Xde%2BYOA6rfxi9WZMcX0hRIyCErIdoqmdFol%2FBdJN91ZsaQrPxauhQlRi%2BfzQBsQa1OEPxZ%2BRJG5sgp%2BPpf%2FFloG%2BodqsJQ8pQ5lN8pKLiMD79T9oXLIfqRTmOBn4M1%2FN3EQLa1Xe%2FUAwkkfMOx4gftaQxBmIzKR2tjxAiU7M%2FwRgfrTA3QZ41BoIgutMcUyih662MKiprOGqd6rIY17BZEq1SIwfQ%2FYZj6eDPHhFeknCOUqlhl8vLdtbfJqEUcHn1zZU9JLnIKim1xykfCtVoJb6lkvB61kZKVHpdBLjY0%2BQjccxfVVIc%2Fbp4pLjEJrs2EDcQM9ySjpZa08Cq4dima%2FRYn0GqQogUE3IqRE9Y1zdbVFvmpB1cTRXzVgKbjc428dQzFO%2Fx1gHPf7TYI3VDZ4DbbwQXyGkYSCKA4sQIhQjepq%2Fn9CUS7xf9yp3bXTD%2F35rHBjqkASOUBLFw267j3Gpd8G7VGYxzxi0R3PuB%2BtNtsj9GVk2ag55AyMUKZmripQdcR3WRysZvNjPS58%2FAaG3B76x51pKEvkLTqCEdRnF4g1f1%2FCsTTgfBNRMfe%2FakmuyJr11iEQRpWhysyAK5avNx18BQMLDxSVyAvTSlmWOIB7lydAutHOM6HqDNikrEUZOvzMSxRqrF%2BO6DVr3W3un7avNj%2BmCpLvzL&X-Amz-Signature=8d34b270cee45dd763f3ced29846c8944541c40927bd0a173608b98d3d39968c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

