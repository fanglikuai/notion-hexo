---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QOPTCRL%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T180114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCxDspVErKYxQXzt8cEw6AQc%2BkUbWjY2XDWlav2%2BO7n%2BgIhAN%2FxklsPet5nyyLYlFziNJys6XDkuQXble4rT0XGnF3TKv8DCHsQABoMNjM3NDIzMTgzODA1IgzJVBGL19cp0Q3hu58q3AOo8UvZCPZFniXwMTHzU7Vio3pUHFQgzmG%2BLIG1FhQq2XWxj6jhQGawSksVw07feS%2FiQYnlkpaTctAPejj6zB%2FKDsdbQoTSdGHLcFpgVdFOgaZPZ5xlrMD5551oiL7yNPvSfdOYAjG%2BHF2AHJAZSGW8B5x9vG2Z6IN%2BNZNtqMCL9f8Y3vDDVr0G6RMttZEz%2FfN0dMS%2BxblTBHPf%2B0HtOmciMlztkxObmtgq2y%2BDI9Ky4y%2FdUcwS83U5SrSlI5WjkcUQ3SS2Roj72DHOurVb%2BjGBw1YgpRDNFCUnqFSvfx75ibN98yZaO7Hn15VWSgDRaZTrQhwg8zyDvm4dO%2BrRS0hUM7MI%2BN7N6GKo%2Fv%2Bajdz2mU6G5YDRAY64EzSuZyAYg2mDqhJK41WdTBzOtYhx03ZoYkkSk50plAun6HeyhC6W2EUAgkllDlDR4twjFguD2w%2FbSIj6Wn2MbMIjKgh7FTY2f%2FFGsPM0aFQKmXV0yyZDj3tNMs0RQ9%2FFkWeR%2Fhpf9Yfl3COxYtPkAzN%2FPuNEMkMBhj7wiq2oElHc3KMMlSzRSyjB7hni%2FM1b%2F2qXqCE55%2B8TZh3dDBPz%2BKjmQ9TOuq%2BRXfzxZ%2B7A8wYJug4Fn1kNuRl61ixYh1FTZJoFtzDi99XGBjqkAdDvZ2PPUi0E8IYpKyIt%2F8Eo1U9T4xx4lX8sNkmJOai4chgGqOn5rZthT2j4rr3g6kQDz5%2BSXjWJloSPPJ3yxNRAkLmbgVxsOssVXy767XVNE6rGu4kCAVlnKPr9NjEPxUVUwxMgGHoyEG4EYSwGB2G%2FqemKXdUun4g%2BIdQZzmxbSfySGFBgpxttXhLT7VN2IWE9FDvpz1teTnFePlq71t%2BKi%2Fub&X-Amz-Signature=3dbeab30cdb583d5cdfdb17ab4e31eec4072533b4e4d87b123a77050381acc37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

