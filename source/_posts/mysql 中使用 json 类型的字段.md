---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDCTM4H6%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEG%2BEuw9h1RXtV%2FUw4hQ7LVNt7rEiZ26g%2FhsrZWkpgN1AiBjdh7xZIf%2BHRtQki7oyeV0GxTR4F2Hn%2FBWPz946sK1CyqIBAi3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXfRb%2FVrvfjqYZmOKKtwDcUMYBf3mmbP3dnp031zSYeNsEYYuVbru60sbudOgdqCyYpsXO45Pv9UnClKXx6wjpwKd8IYwkKu5jVYPDtKeIKTDLpfj2ws5%2F%2Bb260gJ%2F%2BowMDPDJ81o47RvywCicgEfNMPte1bsx8Z7r2jVYczK1%2FRYtXObZqjtIccoIyYHVbcX2Ir3yel3bxK7Y2cID%2BY8KDE6uyNRVEgssyhWDyxrdzs46SY8kQVJUn4y6J%2FxNMEOKPquqx%2BFm61hrlBvp3rs7dcrHK%2FpEbPHxkNI0sIE8mse4AUBitEXR3nNshxaYNBqebIkK%2FtdnzTf5TIbiQMYcOud%2B9tXne0C%2BrXstA567tDU0%2FdZpJ0vLpVjjoKuNHz45NCrFjVF31fisJNu%2B%2FyDr8Z6EfeYlAYPAMdD6IJfxBdUJrZUJ8zeTuCcfpm%2BcNtVtF%2BNz%2B%2BkVG0JKBKBZU7vSJz%2FMUHu8gnKHds1iDSdqfv%2BENlGkBPBhZKM05RQyLYQfhW6i2eEoeBXdUqsT7Zp67gH1iw3NOsDQqAGNir0PkStI8baxeKjNjgwmdQ3tvsGpjKqUnOQP6H2Oan6rbtAWaJnbbtLR4HTyBsK8guNxBQa0aGtdvni1tWypVEi4SuFWSQkCM7XRbH6pP4wrbzuyAY6pgEP0ToI%2BkLzYelyo8lIG57qSdoWnoziTAGlQH%2Fu4xkiruMF1M62u3BDDssjxfe0Z%2FnX4g4wjxRRbtIMZko1WlPBLR2fUDEBK9b3O8IOiXTNpCrNadvQVVoAODGG66luy7070vUtGVHMcOXjaA1J0F8%2Fq2xc82qeA6UxJVm9sy3w49oRObHVAWb2Mvq2lCx1preTtPGDcNH%2BApx2JD1QSUPqldFtqH07&X-Amz-Signature=f80c7ec8c6a5097636593d8fea320af51594d9e127c7de2cc009f06d174303b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

