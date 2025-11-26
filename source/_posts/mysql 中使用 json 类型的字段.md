---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626WQUO3V%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDJhZdrpbSVrlC9T%2BuilwkFo2CenQlVR7VGmJX11gQb2AiEAvxwCqSfJUj8sUHOX1oryIu2PFrwAvdW8hnlkWTx2bHoqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBVWtVCAi%2BMod2P%2FWircAypZuy%2BQpUGLuqIgIxjaWxDvIVdbDvDjEK5p5jCuYfep9dhiItyjnDePsfHJqVNLAxbGw3OrYqvqT7nBYfnzqZ7atdlEik22GBXPlVQyKuGq4YozXJBCWldePienOwfQa5gdxpVmVcnGbwo81EpFqApjoQ2r%2FJb5fNqMrcksGArNOMdAvSSZBt9h0DKDBI%2BjSO8PKtNNox34u0It0qqqm4EXRDJfxz4t4TU7g7DozKpwASWRJGAxDIRx0ojR53IVSfZaDPNhm1k6j68j1JobuMN%2Fo8p89XlwQ%2FAqcxwzV%2FBM7UXaIIt2ZhIIQX%2Bizy8YoIXWl5yHi6V%2F7OuyglN4gvrbDVoxUv0OucDHt%2F8H8%2FaSihXfNF%2Fxczuuiekque%2Brh3xxC1ZHYDw6CHUBohC7u6Jedn2GBsoFYIB4o4nE3Ij%2F4S460%2F1WYobuIQOJetX%2FCL9qeVxlZdqDja8%2B9qc1DT4fdSFjSlZhp980LgUekjdKY4BVZxrsVYNGFtiKMODeLQh9OngzXr%2BTPIY3you%2Bdi0iLld0Snyqg51Db7WU9z8dIjvUQSXqeiItTHiDrQcpYwuWPNoRVtGhAg3DRQb8qWdlB8JHXxE%2F254mLhQWQhzRyvE8g4wo90Se1FOVMM3onckGOqUBnCEMZcYHHcbLNehl%2FHdvSMCyOiT8JtgzevSoyPCe69yTpz%2FBZunSftJiLcFahxnOsojI%2BqcltDmgE%2BmTm2Q9c1y75txqu9xSdicTc%2BrPHv11AA6XKa1IJpSA6LkKg2YN2bWNWKCkOedIVXeOp0jBLWDhbVg%2BpsVE%2BJjmuQ9joOLWUR1eEWexpiDYmY5DDVSnI1zK7hMbojlrllZPszJUcRlqoC32&X-Amz-Signature=bc87fb65ad93a03e906d3255edd6d0959c23b5fbdd03d95f10619fcb2c58602d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

