---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632V5CTJC%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDc63EWCU005y9Dtw6iQNozxUc6B5pi0wCIDRoJYdoHwQIhANP%2F%2BuiSH2OA9f%2BvujEvP5dp%2FbcAgRIiZVnKN%2Fh4qQpYKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwByJGWdc8Kk8be7Ucq3AO3GIR2qWX3BXHRaeAo17Xrp2UPY9riN%2FC4y7g3TTANOKjgRIIjyIHLziPNFd%2FuGxSj2eH0IJglMTiIhsMr7%2FmUbL5OqHBiZ4CO8i2OYPDU0p2HYMy7%2FIWTPeEONREenChZ4lj8e3VFOY3MnmB8XyEGk%2Bdc8bZoG7D9DVtEVr0nrFpEyT3J0FbvpcA9EtFFbq7woeH8wY1ERUB%2BZvJzIJ%2BOqDBf%2FjiWettw5dO53B3XFgCBBbX92BmJntVDOXPD53zEROelVzIeIZ7ygN3m7R%2FcjrsnYxkpsMOT0KlEKNpQ2kEToewY%2BZmwcgDq9MVJgus0UpCBv9z2TxgLU5pWElllJihOmQ%2Bv5BFsR937cV0wnnHUG2VPmW9ElfimiW1EzEhM9u0zA5uJvrUvmMWn4RG3tur%2F6jjOMom59fDo%2FldaOLY3TzNnuLlfgj0%2FBZg%2F1ky8h6xfJ1DRzCYHlSuU6TC27UNG3sHlTsaDAt113Zy%2FHRRLeRpBmyZeJXE%2FeZJD%2BGlVAsfhwGJ7fYem%2BE9%2Fvx1rQrNYrx2XRNVTMg2zEZwFae75j22%2FTQq0Pp0OTLYjDNpm2oikGmbX72bMTASsa1hljMkebJoevqSo6uTFUJowYDJfV5N4rEXET3sisjDOzf7HBjqkAZXhYQ33H5%2BHKmrl%2Filnmh4O%2BFK7t1GXxODNs3QVJWaKfOTK6hZDrbw9PMDIFIjOPnSaUSGRq1b81SBwQL34qGfXReSC3CueDaBxJVR0bhZa54qlSrTULC9bI%2FUs7bvg8roMeQGP7V6zfXdfp19KqjOb4Wz%2BWK9XXruCsfwOF99f0la3yjHbm8DRqRKW8%2Fqw8IJY%2BmvBDL4%2F2H%2FWG0jgFQRzuARW&X-Amz-Signature=de8dc062176b249f1faf9d81fae45cfa67237766619617a510aafd120216c8fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

