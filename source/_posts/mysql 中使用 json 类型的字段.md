---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7MD2BYU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQC70BHVX5UcYt4xld4Bq6gkdsyo%2BQksM1IWliEXTRFDUAIhAI8ysHFy2Q35yuww2%2BO6kcEMJkfjaAt9Hd7vf5eUVx89KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxhnJksQuOkV8mPdJAq3AN%2F5IZ55Wxj%2BigJUN5Tm6tpbpFvyQLX%2FmYNr6ZeM7japDG6LL7c36DCnUc0%2F3612eupqRed1XnnJ6n0RG8WEvrysb%2FNuqHYxSHAAOWH1tvnwhGWqox%2Fb3SayztEmYQchKC5D3PIr%2BhlhMH7KZPMpWUqdPyR9g5KAmnYjyzomK3Jut31vHOUFhWwGQTb257KSIdJsWVuXFskE3ajldHL6fFI6Zhepb%2FdLeVEVGh1Li7D3NDTW4E97kDgUFIv7CBOsublADWpUgKYYUmT9SPAJuZkXWjLjlG1sW1I4FkMkpMpkI0niovAuUYcyhRJ6yhQDQbTVhIp6ByOQYUg9Dto2sdilXACz0FAwIzHugvkDpe5SjAEcDtg4ROHPLmHwuQo79vkMetZ%2Fv17nCCjUY2n1nE%2B1u%2BgDDlVAjkp%2FshHhwZxA5E2mScvDL9y3cAheYGrLW9D9c4uyUupkm32vrLj0tLQ%2BIvqDLvqn%2BMRpt3%2F%2FvCpCQfhuOvoVaKJGzLk4yYeJDB%2BKC%2BlhTl6CyJ2Vz09Ad2%2Fw5KZHxwnV%2FIVWjjwyYYZbtoPM7OrRy1Af6uKn4YQGgtI%2FMMB%2FsugqSIDoTqAyYsNVkAZ%2FpFLFfF9ODDr23eZUwDDaIHmRBrjikJfQTDXu5PHBjqkASaI%2FB0GYwFMY9FvfkJ32N%2BENxCyD5ZwZs6vUsctskwDzSlMyju%2BkFvNzxnJ243HERqUVDxqR%2F8%2BJm7Nxi5qjhe%2Bzrg5ML6mJVThnVbJd4g9%2FeARHvmOe7PqHPNoSTDnmSMGJqe10Au9JvKBR%2B7jaT3f2Z57e3ak2HYHH23R1aaMSTxINiU7SqUTzaLn6p41eRnLDZynLI4xLmquzF2RwbDajZVi&X-Amz-Signature=c5922cba7a7a6781b32c3cbb50bb048aacc7b6d7b23aaefb6890032685cf424e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

