---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VOLOQYX%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCwA9D2xQn4Bbktly1eUcQqgnGwvW%2BBnqu2VWzfHvhARgIhAJd8WQ1Oon%2F4uA7tAXRRspw1kDeu41CeLnWaafPJf5cCKogECIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBfxRxV6BdLFjJjuoq3APG9YLhirfWbcWlYoALI%2FjhCOz%2FdYOe2feHS4Pn2v91B9EDNstkeUgE0JpC99sgWWz6CsplXsvBH%2FC7zEew%2FFNAtu2OGICb4l%2F5HgUhC%2FgcjnujAuXmzifbW3QBdHGl0XvHb5C%2Fk%2B40GYa1bhyq2NA60voK92UIHXsIrHWHW3DtktMRZJI9mKxjrrYEDV%2F8PXkemC503qtU%2B9jecPPgD9vC%2B3qru3vdGVYmSuIOvR4VxCpX%2F3JGstBQyqKBrTI8Qc0mJijJ6PceglnLmi6tTwBgM0wpwTslKQA6uSo8Cp2oH%2FsLsSWmbqNu4%2Bar0TBmm3RsAZaXaVFee9lUQhtpGLbt0U0LvKoeLNo0qtGm4M6ApjYsmadzwPvVpGbv%2FIDw%2B3jPF5dCOGJWR8eeozRTVtF73RrnUdYcmoCA%2FqwkyA4BJ7ToPQKdUt7rmEksxA%2Fox8u2UPP4nje7zU408EzQa4CAHiDwGeitqTE1fJfio14zwk%2B6UQa3%2FCDMbgBhsTfY18oY5yBgiY3PMmiL4QFswnhPZZ7Dl9k%2FKmfF6%2BF1zLKMUf8Lt1rASUYNUoJur61FpEFM44swBVlV%2Bxs8Cz6Ilzw1YT7hq6ABE%2B4wKK9MeMvR8uETc%2FAj4iVhRCCCMDDSoKvIBjqkAQeHOKDsN09alF%2FHAFuZYLN8uWRNHzFxm0nqmPKH47ePhJZVBn37nsjzev1C8ypYiEgaRGvzw3V55RoztR%2FZ0G%2B8hw8bJe8MZwXu%2F%2FwZqBL%2FP06C2%2BFJ6gf3W%2BOCQtEdIVQvf1LvI%2FAVMXQ9urLk94mL6ZhX0wcYCGTaG0vp57tNln1Lh1e%2Fb4VQ%2B%2FaIrHIa5UuPufOltKwWkviasdLGov0aU4bV&X-Amz-Signature=5eafc9ff2bd54dfb6cf0f59b34ee3fbfab999a7b7b88ee385146a620b2719f80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

