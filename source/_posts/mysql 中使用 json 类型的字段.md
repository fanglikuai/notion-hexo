---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFLCBMRT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCGsUeCXl4%2FAm3uEZef0xFWzp9Z4Z2jTqOMAjrViVRlZgIgQ%2Bu0LNhVYKSnxiDcS1IQR0v%2BZX7Sb7uOZ%2F79CCFVF5Yq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDMhUtp3iCNtsxdUJ%2BircAydM4ozFaywhs%2FkObDjm5T3%2FK7oZM%2FibX6r94arFs6pNgOI40zu5%2F2%2F1aK4geE6rrBE0o3GMZRJFgz9SN5Xjck44C%2Boym4EfRbIzxoW5TPBtlvHtfJcqDW0mjIdmBookr1Xli72n%2B5wVgMRJtNG%2Ba1qRS5VjCF7DCk0MH206Bo%2BfunAS1GwasSMLnjxNmfBwC1z2ERWKbjyStv0%2BHLHrdjFuXFVR83Mh8eFo1u3hK8HjuCjU%2FxE8ErhUvk49VCAuyHTC9hhUciuMTWdfQpguRaEF1GT4%2F%2BTK5beiLJ4sRkZVCOuKb6gKtgOYcV49ocB7tdG%2BHk8NZ5le7kpEp3pqFXP1AcUh5%2Fx0HWemIXl5c7N3RXCVvZeoxIRX0vtq2%2BUyoxdKvI6SYbx86lE2dnLAcQBHRnlLB0v95qUGuDrVEaqEH%2BLxYeZn2X%2BbwI6%2BWTAOEoGt71kGzjmh1u%2Fq2MwHX%2FluColN2SuYfnb5nKTXmohQdMAEUvf8TPcRMihZP0AS5yZgZ7wXR3XIc6X9fP%2FkLLE2TPiD6qMWLEOIAMoqhSQ%2B%2BpcC%2Bi%2FOdMdKCAROXoSMMgOf9lDy4IKd7E%2F4GtQVU9R4VPd5w4xZAlSBNWjJBF8rCnXDOte97Zn5KtxIMNaXi8kGOqUBQgeIdgbEgm02TY7FmJh0cGbyPvdvtBS%2Fj1J59mfIWBK5Hmu4N3UiXhevw%2F27iKVblisDtR6zGLc0AXZ0nt7ERfOLPzZQOi1J%2B7T%2FyZdN%2BX0Pj4V5KKlZLRsvhJOHQGPOdiRBcUMBcL2UmVwcYRh%2B8lWo6zE4SRDCZpysuRZHMnYnQqy4XA52%2BrdfaQqrJ%2FaJr2yuhltU9bxpqLmjbfpSoF6kBoH0&X-Amz-Signature=3befc661852e1ae15e7bc8f6b0e09a6e99f0e2b4b4410ff8064ed927b4c10d3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

