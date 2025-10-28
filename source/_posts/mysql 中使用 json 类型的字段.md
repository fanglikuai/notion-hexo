---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YABPNZSF%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDJkYWLuvb53uIESQhbhE55w6sVYxoIH1QjWHaNqTcYzgIgSWhi58a9bSUMnbVWJdE1vfHMRq33k%2B3NgUW8XPdLKKgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMASSMhTkldupvZsQCrcA2UjQvKwI3bbCiv%2B49djzJmdipMcbMImvkFF%2FZS6MQs0e9PHhHE3r3Z9tnFYU0qfA9QwiZQQ4MelZJKl%2B4%2Bw1z0LMVoICt%2FChaqr%2FyR8i9hQtrmX6b89McF816i6eyU%2F2d6WGaTar1dUGSMDWSf8Cxjn9D%2BToAr4ZqJgLHsi107zlq9aQOuEblya91b1nP9wn5wMQababF9qDTPMsQNHNSyvOT0fZL%2BsoO28qCQ2s7FTO1rrbqrCjEMVNR7gJ8LR0Y9OonKZHRJ5vp0hMdLLG2TDsTmImHrQYkvEwDn8%2FjSmL6Se5nALD3O0IPC%2FIVfKBFQAMzQxq5C76%2B5NdAo3jE1DkYpp75CvddpP1HP0PjThODHuwQeL0R4HSwMXURhG1QHMaOUyIaMu3YywO9cslXBGZi7eywpxxNZZOPCTfKsfU0k1ECI6XA38saQgFA15YRlsDwP95KUFCm%2FcgO%2FP7U34SZK8LjihQsMooYlSuyfvvHIN66OqrddfMTdDtVBzN62b6T0gNIcIUxc3a5j3lGrVNSsJ8ga1kqdRojAvm7rNEQCnT08E%2F2mRTYlrsaHwHcSrrrqBpRoEfyCntVN75SKad9TMUlldeISi4kWAB%2BzrXsHZlCGtYlL4jP8GMKOThcgGOqUBmU%2BO5xt1ArCkh08hAaTAaK%2Bs94tf%2FlkXzG%2BSHcFSR4KMWm0%2B2beXYb0NOJerSgyc5xr0AoxPRCqgkfZd5zbeKMnpQqA02pwBNiLVNFgG5me0pSxgqWK7kQVLIPMjcFK%2F9C4M%2B9JcIlmgRKdI1eNvL0xEVeJLRnIQBbBYe%2BqlENdO8M7RKiv1G4H4oS1VK503s4quF%2FoWNb2dcIGe4zGfPdi1h5hF&X-Amz-Signature=cc2d9206ff07d4bbd33fda40856012f5b6033ca9ab3989e0653b56ea3c8a5849&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

