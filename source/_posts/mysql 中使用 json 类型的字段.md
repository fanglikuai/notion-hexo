---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYX2OMR4%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDU1%2FJlZbuIBeR%2BfObX8%2F5p9HAk5UYQZSleVqOJLanY4AiAy9h7KXGgGPu0hbQ9FsGnzLFMhJTMRn5ZsR5%2BCubJHhyqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMgNsFuKeTlZmglRKtwDvHbMUfO8F95a3OhfxnLKMsW3ySJnNckNJ9peU6%2BxvTvE10IoC6GRGIrTfwHi1X28u3WWB8rQ91UsVH3lt21hTtB5vKSHcG8Xk2S6p1U8c0VqfOAuVwLt6JCUSSSlhhFJkrjlrBC8kqy3epFOfC7AVRiMy97FQ3sTqpbQUPLCMsq5pGbxWRquNMnd3B0yfubLh3yCpLGw8Ta9TXyLhLNTVYjET4f7vcziFSAEeH%2B%2ButSrk6ocBFWJuWhRp7Ka1LvOLBsWDv3BHlt81IAnrsZFTCv1dWm%2FynEDTYkRYvKgJVSFF0HIhrASkbTmJqiOKRD3Z0NB4Y%2BcVA8hQq9ZeR6xsAXnqlMZYy9x%2BjUwII4sXdrahNFeZtx3UWiD5eQUHgPN5Rf%2BgLnhCFrbQ5LH7h4aXKvHrxA7q2tvm42E1qlqCyT4WjQYKz85mTIK6su0zjTi0PDBIsK3T%2BVBbnlef19%2BYbOcjm6%2FYPr3%2BfCdvtuGvycNzOBPLRP1sEyGgdwoEkiGMo86uTCRTIRFrtfNjOr8uFrIRDYq9ySC5%2Bq%2Bu%2BFVeQWX7CQE7ilOZKYs2nGv21lrNnvuPhYVqX3BpsLOzH%2F6lEJsYw8nPbQAZoTYnTpRWSzZrHNw42b6q%2BTbpi4w%2B5uAyAY6pgEcjWrsxUpxhqd%2FnL2YLiB4CEM0FqIK0iUjELrX%2B8n3D7orh%2BmONdieu0pAOcFHhG63Ga8AvTCpIoIxIHGvHems4g2TfSBOEe%2FWriQNQ1GK7VajKRvaQZ9MSE2SuGIWLg9%2BQ2YglVmeK44l9hHTH76sYpZ50km7LKB675o%2BifLzhNcKvYHTy5y0cgGI2mZ7WahsmNFAX0dYATYblz1NmVo0bNCTSSr6&X-Amz-Signature=00bb151061ed9aa863053761f7b68908cd7aeab156ea798a4021fda0fdcdaa76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

