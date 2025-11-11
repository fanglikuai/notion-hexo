---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OR3FOPW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQDqZ4DFg49ql32yBXfMZBLb2ERkQdppgaTfQGVhSj%2FuwAIgNjeJWmrPAFJrOEIJ%2F675wXwO%2BBtE7rggaaHoqH1ajogq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDJ7E%2FaBC4h%2FB%2BRXIOyrcA378avRf5akymzWryEk31vj3kvbc8dCc%2BjS51qRN2TCvUDICaGFoc94Da9zFA%2BKe0jafvI%2FLsUZmGfC9tUGOHovxScKzejIXR9%2BLPQhRJ%2B1CHaZlXzF5HwV5EzZ1G01p%2FbSo4ezh7bOfNkoZES3uKZZ594T3wK%2Bi%2F36IMFsvHKZWWucogH0mJLUQkwhiGPjj%2BIdA4WrerfasxGMyYRHcbuDStITkfD8MmD19tlMO8IwF1SgsLQ%2F6If6gwnwquBFXXFkPmx1flOH62Bc85YczmtN2vXPZRXzv%2BISao3RH%2BoIC9vE9MtffGxmKp%2BsSd%2FG64Qb3bCES31ZrDPyKb4pfr6D59Jyw%2FYlcr5%2BU4m5xWljNrfOEDwgm1k%2BKUP9MHNJeC%2FhNoLEF68hEVBoptHg0rCI3ym%2FWZcw%2FUAiMgOm9N1lWidkY9M2VWl9au%2FpI0q0O1jdMV9GPktSRMFhmecqO%2BLcUg9I8DtR8oMsulfYfpydnozYu26YI2BvbH7bds6COO0r10hYh%2F5aAAxmCvvX%2FTmKZS94ZhflwmnbwzlDSgrwhvp7SqogsD2nPXiJUoN7NUozmfvVt45IeXzbshJ7OWB0ljrNc04AFrcxMcpe8VVFMCfA%2FnBvBYBJcBH9iMITpzsgGOqUBx4%2BFGIk4%2FtKj7RVEMHHjJtNGDokjIVFmKM2M%2FFe38Df80XyVlasP8v7f1P%2BMEOAEzZLY47PTh7CqD1kxcCVCfCEWFf%2Br%2B7oh3%2BAlQ6Y6wRATZSOiR%2FarIkuZ%2BrwERwKfZiKp7%2Fq9VvzVt%2BVnE97fHyPA7XbcKu4doVWtBCJLH3Jc1hYC6t%2Br9Joc3%2FsZCX1BTIp%2B0m1EJ9x5g%2FheKG0YLOZRvAJ0&X-Amz-Signature=564370b5571faf79d667282449b67bf96a939b26ce8542830219b615a4a8ca47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

