---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TCS5OYZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQD6SPWqaXVSPWoXirfRKUmqlw65nk1hYiaT3lN8QCfR5AIhAJtVlZSreHvT0JV4Urx3XnTQBLZOZdk0fNU%2BMTzjPSuFKv8DCC0QABoMNjM3NDIzMTgzODA1IgyhaLcAZf0HbPWatHgq3AN%2BkwJ1QDo5%2BQOAepk2k7cksBuloCKVBqOoALPez0TOobW9g4hm%2FRoymaUcAybxRleoBqbAcKsuhWFFmfgu4kdCw5Nk7OLTBcb%2BQnfdFsTITD%2BfywzjttbGYUwkpKb6Vts7EWE4sXJztITbVgWEbCQoCs4h2V%2BEvNfv0Mpm2hzpWMzJfBIix%2F0DekayjQ4DwoHQYBctSZdNfxUj8pLc0lmLKNRk4kgWa%2Br%2BaqJa7x9tGnMOpNPx8qntEoESlz%2BnyADs392XT579vJZmQxow%2Fo4dCmgTmqWlDRfXyLRrgqM7T2HG91o4yQbO2MYMw9zGpiBomHKXedygRXWLLf7OCMDVq81ffqAX8ORIXwlLxw%2BchJN40%2B0WX9rLzPZOQF1HvM%2Fspgx8WadLzZGyIo3A9WCgNEAGtFNsHJYAd6tLDpZ9TO5hVGhJzEW7VCWB1o5hQdv%2FzLdPdWsCG4M3tYMfY8E%2B20cp1E4x0l%2F4M5NWhoXUnLosY4cYzo7B53E%2Bv9p8SMQOIDryF7gWJrmZ9O0ybVnrvAgMT0riyLdgZM2x6pzuysAQoDMpNdgttdojq5VpF0I4Ft3YnSq2%2FPVPu2ywT%2BjB464aMjEO1gsaP%2FGK7H%2Be58n5aXcXr%2BLYRrEPUzDv8JfIBjqkAS%2FZ9DGb6u%2FuWp70qhoIfbKMc9GeUd40b21s1J8EJ5Nl0drrYHp1RdCVyLz9KSVkt7zjjdTO2%2FewZnl5htVr2T4d9uePTiK3aY0k5K0DQIV1pHTZhoLbUq0i0Az6yhw%2FucSOrMEJB2Ogsn8NmZS2ar%2Bejsr7jH2JkL7s8aCa1rMi%2FS5BEtjy87u0tmYZfBujM%2B5HKVuSbCoO3NRNzJIC5SmzhKic&X-Amz-Signature=156950b011460f6b420677d17b17f2d850ec1043b889d6ab895661e12c7a9f84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

