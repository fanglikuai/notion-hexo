---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULS3Z7P4%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQDn4tRr1Uosmg7TV2Csio76OeJ9R0jNd1AVy0N830UNNgIgTui7dyX33rFgjhUZsKawEmbXr9b7bU3GtFSnp3UVTvsq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDC1%2BoJpwgeGw5EKEFSrcA8KmvUZJBbM4qp%2Byp7hjNpm5%2BVHVxCA2Ynj59PH42keRaqTbzbuVXf%2Bekf0J24fj%2FK7SszbG39zoItXbMUH%2BsOS1x1s1WRc3LHyQAjI9og1wRYJbLMr5r7B7Ut6cGKfZEYXL9rfhNSyH%2FAynrqf5tcdYeZQ4kaZf9kO4%2F0p7qlFeBkCuOoAfch8YScO8VsDT%2BSYc5QBNuFtwZij9goJHZL6NHXcwIovIrAOLa%2Bxp8GUOOToAWQ2o7G3H8OMNBZNoDy0L4ve0IigxsUUMtFCA7%2B0iUKE%2B84kgvRP7pYzvOE3YOEzHYPpLyN449hpt0qNlss%2Ba2DdDMXLVJpj1iyohLzHzEp9MZ2BWlhZKdB2pKzyB4gR1jZezYxjpldpyGUOFJaOfrEdJyP49Oq8EieHkdPMR6P57lCTC57F6ztMmZ%2FC3hQDYqMXLnLDJn3jDBulG9ye3cgF991c9NAKkjQCw5VQ6YzMs5RfFcmPXDaApe7XhkJS3wtlyfr2bki%2B3yiWVivWd8PIPcMQUOf4MKixxy0CcUHp%2FAI5PuRWI43eYbhlzEEo3XK%2FNIC8s2yYGqgXg91fUgD9HfyzEjPAxmq8egObd%2BnGoePptj80HfXmR2UjzoAGpdAaZxPnCNvOkMMrEqscGOqUBn2d0Du%2FynyL8FliaXJnA4nor5TZ2BlNuwE2mef1v4uOXIl7S4sFMptazOnKY8eeyR5KoDZ36sbR8zBMn2PqKlGoClSIk8t4DvYGiIwdNSyUfkcVtupl2ydNmSjjm6auu%2FcR%2FGaF5T4Ug3nVjgpU1kOznGf0mO47mSh0Eg0pgip9aW%2BZCVzn3UZI10V24yha7lKIOeMPWAlokRcyAYoXBKD3SEIms&X-Amz-Signature=fe7110040d8d33950c6d96990704c9152d28ff200109400d2ed80202552d3ffb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

