---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVIY3SXV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQCqyi34dchLgUs2t2tQKsdJs0k%2FR93YjnVhOQIWsnCmEAIgc7QzoS4G2vjzGRcJTVMmIG%2F2D9xFXLJwbuYOZ9kgqH4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDANEN5Up3q9D4SM%2FVircAy3OxsuM3agtrHYeomRQObk2aasqx%2B3Z8SUPKux0PxajscL%2FGkRfZbkCb6DYpp4s20aHOUYj6ZuIXPVsxxl4asXkAy1gCVVa%2BeXijt7nYFEjPVEeQU5El4CEjw%2B6oyiO%2B%2Fa0p7fRr0eGo7cyIL9kTKBmmg2sBYuCLb09PvZnmqskiL48VFUJ74qLBw8i5Vxzh0H6lcc%2BLhM%2B9WwMiEzL0y30MuqS8B0y1nd%2Brd7lG%2Bzfalh0GzE2ugVp3P%2BuzQbRTsg%2BRjYG%2FLBw1uwm0AzEGq0ilNfCGSEaWJnVyZlhv1Ajyxh1DASTep5wuzEiU7n2R%2BJTo8hRUkbtAN%2FSz5nbmgtvO1iOP89acxUmwHd5YcYDR8zdkty7NmmHGEtcLGTVocPGqRjPI%2FCUeJ21FFrwsJRIbvR70KkGyvg4zVdaBk9xlBC0BitNM6xdUm5eeXs%2FikQGHuiD%2FBL8yRd%2FbjqtDos7oimglqYkyPAQANCvWf1k4M%2FkubfLlcORJHCyK32aN2uFxia1PSEUCxCo%2Fd1aC0RPqH9RpbquELL6CMaoigRJhAfX4NjHCdfV2sC9WF%2BHUOFM775VwL6KA5F9gGQHXjQaFtLPU3YtCRzIlK7E%2FKh3j87GtPVRAentsvGTML7nuMYGOqUBANtsNoGxFFcaOX9sTu3WGt%2BA%2F7BXxtkwgbu24Um19NkMZWo1SQ6zNJfhHi%2F3LBBeEBPRfzdJfdP0W3wkbF9R%2FBZRgM8zS21LwedzgEJ4w5VIi92%2FusegfsSpw82v9FGvJmTMzKWMImjJIPn3S%2BM4Wnei1UycYXzPcGeafBEbkH2S0%2BugrEFuF22wABqm3ugyO2AB3C0Y1CyvoMjQFWv1eN8LYsau&X-Amz-Signature=608e2928487c03aa2ad1cecc3e0d0360656feded7279dcd24b55bb57f274d298&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

