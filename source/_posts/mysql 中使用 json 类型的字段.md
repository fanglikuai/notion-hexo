---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665UL4OVX6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNRw6x8bAdVGOkSVak%2F8LGjKzlYvf2sHZGCtsuFXNwaAIhAIBQd1nGQuCW0KuGhLQq4Pr9nF1AwwyiOsshc5tOPh8PKv8DCGEQABoMNjM3NDIzMTgzODA1IgygGmeKWubAdmQxaqoq3ANQrsbnbwPjbdFIqLAc3EKVasVu4tRXgdpDJ0cLoKMDypFzd94osT1VZASE5rAzzDFKL3tYBwQVspmTQb5XJo7MJv62bKEkdHu4gtIbqnvEJPU42lijSi4l9kIGdBgCgv4DFADqEibQFgqveUa%2FnpooBypAscxZ0UEqZk0mpv6z1zF%2FKjsLS35YpttzyyeGFiOnsYSRQmj8VihVdr7I9%2BZGbLU15dxNp%2FR7kiYPQSjZ1XnX9LlLwr6agHSxIhAboMB5FvlCEl8Iw%2BuPVLOuoHh1KIn3M2W%2F4Kz%2Bsb%2BvEGpwyQc7C1X8LDwJaIcdQtGd5i4e38OK8zaYBWQbRozbr%2BfSjdzHACybV4YvxVa%2F%2FDo26l4xMSoFyKHu8ct7MLoZFx%2BJtbi8EdlAQEMuK2O3X8Ekms3NpnCJ1M%2BaQa4Hu7TLI5%2Fqw6VXc4hNa0glIQqpbqBpvH5GCObM40ofK2j5N%2F8H3g1qWfTAB%2Fjuk7R1qOvCVskF6EPDPhJDSaFq6midikimaiKh90BdFG9BGkZKDZ9UtSfhk3xKHRfC%2FDXT6DUxaP5MR0pp3NgqqXe0yM5SpU85OQ%2FTVeEjQBrWn2wxx7sjqQZ2ukFlcfQMg7MaLOTEU6rYRLInTnOoE3lqrzCTkIXHBjqkAavIK4FnUvLyYa4DftLGJJFf4LG%2BQUI%2Bm%2FtSYWCkpnA0ikgoBYKrYCvbL%2BiVtiv4dlpZgSiYUPUMq0U9Qvjuw6lZDg9d4nqtaea2EcmnN6iXvCjHD4CwMkf0%2Bo2Y7UmqbGigRjIFWlIfna43H4OT%2FR4%2B0vCpEfbbVphg4MygIHTHB0qbNV2JHRZodGeTb5HMpuDOqhN0nSsBhI0GFg342n5Zuifh&X-Amz-Signature=132d8c785a13820cfa5503dfa4c16f43122f354cf7b63cca79fd040b5c395f58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

