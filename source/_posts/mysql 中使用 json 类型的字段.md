---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPGDM3AM%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIDyPoZOcni%2FjoCtyoSi6ijPCJcWKmb9cFptYJg3AOcZ6AiB%2FC6HV1ANl64Z7UZ%2FaEfpL1CKzoC%2FYg7rIAl4GBocMeSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcDFdZNDUIF3bveb3KtwDi%2BwKzsgQSKCTb6pSbynevqJ1H%2Bk7P5irY5baQTddaxGIvF05lmkWKrKBUm34%2FFwR5789huHZ04SkyHdUofZUUnWf0H64BvTURYbLoA1i1KBSjhGmB0YxPbHY23AoJsZ8PbjwUhClZMrHb68GRzgAU7hQMpfD5rTU0g6gPH%2FXEQrdMdcau8Az%2B%2FpsHHRbuuAjNYvo7RXzAk%2FlTFlVwmZijoMTfg0r3jIh5wO%2FOnIcWFOVl84K8LzfprwKfaG%2BVdnN6PU25ZTYjBl6vgvFNOGB39XtMYtgKX7dI5Rq2ifsplM8UbAT7m1iO3czcSzaV0U95EppvwLR7OCYGiXOSmJDcrUnxN07xsrr1BsESFiPNYsmgSpFq13PzwCCPKQaOD2VLdBxcp8YJiU2ke38Uk7LOcmP%2FWK7fn3Vg8zBMdJ33gbidn1kgQhfun6zNOaNXDbZYwdRbsYOv2P4LGmYk5FIBzn0hPDbR%2BaikGgUonbEGOKlCm9N2J5SFZZOnLGTGQSMO2DM70wQKosFB6ZCQOQAoSCLuBP2n9%2FdMZeH6JHhuY913icZjpkumxrlwSlaZOM5oC1Ay%2BYLAbcrxQnKddlnIzXw7ZxzwBXLM1a5eZrLoLFCixNiiaY8S2eYB5oww9WRxwY6pgHftiEz2tUOuAD97rrDxuz5UPOyUF3W83f9GeAgVmaFm3wqQtGt73ielpPH%2F5VXEhUb1psC7RatAiSyyMHka4eVjDReko4NnIAgs14znDrXlYcvGwWbYgqMWXIMHEHwlJnwum310mkH1KcR5j2Jdo%2FDZedca3S%2FoKB81jevS%2FhCazUZt7uUv6I%2FH%2Bq6kDVr7nOnSA0dMu8sQ%2F0LauvMT%2B85l6r5J33c&X-Amz-Signature=77ec644e73e8f39d84163c0cd3e36efbfbc0571fef2da67b9da382f1dda6df2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

