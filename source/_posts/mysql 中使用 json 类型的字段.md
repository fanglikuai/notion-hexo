---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Z6OJJD5%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBcbEbUvw6pGyxiEDLL8z7xjGUQ9Ze9Gj6bzzoQCUTjZAiEAo40uoe%2FRsiF98lr%2FymUZw5KsQPSVxYjVtQ5XdYFPFi0q%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDHI2sE3PoBJ%2Bjf229yrcA35A2YTk%2BIWNFtnPuMzZJ2RKlawuCbtWQjojf%2BsE5wNazKUHWaOVfPBIGml67IbsxfYbcmZBjYQU%2F6m1jRy9YKf%2FX2SxWif7CDjangoCKYB5WEdhi7mK%2FBmFSvw8VtOnoP%2F%2Bw6%2Fho3qox8OncUdsoXVHlbN8B7mIPgUQdwwQePKEHYX06eGCEvOD035ItJK1MLG6l2H31hJQHQsWkc0fV7z%2B1mHwOADSono66i8UvAWuso52crDUTPeA1owG9mIGzc3DPOvWsCiOS%2FSN1GEnLzJCAK2IjBK7rIAsEN4K1lA%2FXur0o7OJ8k8RnpJZhsYuxsYIoxO7lFp20atiZxHwEnUzRwQ7TRHojvHO%2FjiE5dmWdZFA04q2PaAHLdHlRlaBYGSIhrZKNaHVq1k06VjqotM11KmVGYZaVHCcK3Khwfz%2BdP0GOwOh9xRrGzU6IE2Qr8P5NDJx4BuJBedPbqkzwxBdReQYlfUmP%2FO170%2FfiHAr4Tv8oBZbTFNEVGq1M%2FARsQl5COBjoEz6OPRZl4nRX4yQXQ64jj1EVmftT8l9iETO%2FrbuY%2FsgRX88TCCDTn%2FoexY%2BfHEtmMakYwxTSw9tdBIGlpKijT63pkgF3Co5na5N5sCqNZqA5%2FeDeRvFMMTNzcYGOqUB7%2FGXl5dx%2B5EayqPSRLCvgopJf3Xe4VtUPzNtkBLAT4nONe4QyyoOgmWfyQ3bDHAFv0NNhJ1yDIeUtpGz9ldq3URkdwbcC7Yvn3XuC4vMKxracLSYz9lMgXc6rjkl1nWdVtww1sbfdLQw48T%2F9nMFsDxhdGaM9ogQFZ7H9deY%2FkSVaQAJBcwQKnkWyJtXjBMjvQtPJsK%2Fm9LJd0rhKHFxrWqG3E8S&X-Amz-Signature=5c41976f8dcda350d177050acd15df0b3eaf1186e02eba72936591fcad9c6fc2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

