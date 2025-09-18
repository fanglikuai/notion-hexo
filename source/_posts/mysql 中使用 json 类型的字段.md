---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SW5QCB5%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDoz9aWvnLCP2e3eqbz6agMVyQz0%2FA3%2Bk6EF0sZMv3hXwIgSnUcczFfEG%2F9xySepsQcHyBzZ9F2cx%2F3nXcBFlEp7m8qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcDC0v84M48dQBmKircAxijXnJKBrPn6KUpl4r1eZ%2FWzcK%2FPJh2KvzhwJKOqb6gFARuHLDNrRxf7MFfuCynBflD1H7o1sGRsGQiTNVB2T3e%2FTr1VahZNTuHaTIqbey7vOWiWB5waZ7VM6%2F4Yd9Kur%2FY4FuSAu5W%2FaXTYWDfWxds%2F%2BHnWDHDcXw%2FjknKm0peXhzpyZUpVYJHEJ9s3BMrA13ouZ09aRhouXT5p7niQXZNR5Pfiv4qtr07ir2OBeNRqUAKNrQyp3MZNc3cqzf2Sl9i3fVR51ZzD8HfZOMeauXXlo4vX8rWC1rihrpjGG7dZD%2ButawgDygOvGO0zmnFC9lZZgi2AmnhFdqnaXp4o6iuiFZQcxDadiQtPs33DNED4f6w6cPNxix2esq00cS5PBEiLYgtMAgk7xVOo15k8Z9u%2FEhNPFOltwK1DMoXEmIDFeMV0cuvfaQadoqCPgiyhbfcG03MSrDgoVUCGgnoKafwPWooWtP5uMoU3mijRYhInTilA9J7tKx2fPzpVqgnzpd6NaoINaTrYAYwhCwqfF4AO%2B1DbxhvwKYb7Q2ncFbRQE36C1nGgNZquxnvMhHpyOl2IGc4F%2BuozQGEs2mjDq4%2Fbipfk0fuBdm56vMasIA%2BblXqeVvSD1tGFzfMMOKYrcYGOqUB%2FUA8mTBtd6muXL7UooEJE4r12BFKTeUhFJzMY%2Fae%2BaoeacRLQ3ER7ZEZDk%2FF47ZcaGBeWwxEIVhsVH9qx2cSh4E0eRSaJnw1g08t6VZE9KDRQte2de9OR5jij266Eq4FugxYt%2FqIDOKWzTwCfhG4RCaNe7WIweIbQuIevP1iRHuusUJcuruLAxNwQsjC%2BPC6JVXOs8dgGO%2BiTt5kKDjp3F%2BlTKHo&X-Amz-Signature=6bae955ec2c12179a1dc18aae66eb60117858051aa8262cf221cdd6f3301448e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

