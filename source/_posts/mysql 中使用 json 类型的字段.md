---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2A6VIL2%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEOT%2F3DFFVPfehQjbehqAImy3lXA96%2BIg%2Fo5v809fT6RAiBxST1Qc%2FYfrJtPU1ZVnm54AapD1L4cO39Nc%2BXC1EsRiir%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMJDOqndOVNNXTBZYKKtwDuteArNyQp1HqY6%2BVziO6l%2Bu725DqQcfW00GOHfNdviqmOmb9ItkzNNipelPRHwWBCyLi1P8jOMgaLw%2B0Dt4MRKJRtPFV7oDynuDV5E0qQAdT0sBgNNRmP%2FkpkNnoJKCCbvHpW0cotnk4DNLeFA52HOBh64IrBb%2FnM8GE%2F19gK%2BMVByIh6TvQ1A%2F6o0DLH7oE%2BUTt10TxJJOEijsaQ38hNsB%2B%2FKkbZD88M3HeVmetodLClEVuByuMYbgo0wwK%2FuFMxyl7wJpLfwE8WG4peCPKL7JewE1WHwhwZUmQf8%2FxLtmGWMBkY2L0HN%2FmjD%2BoomHyxyakyXSrOiTefjtGW%2B9Sr61tRJlHE%2F%2F4Gtxdb9A5%2FY7M7EwxG4%2Bc5mRKf8MbIVXyF7aN1VqiOLonoEnA%2B5HvmfkJlSTwfmfvoK5ie3A%2FFEVbL7rUfEtrQDw0onJE37shqCCxs61gw450l%2BjnrVTviZiSPxaOEcc%2Bpb3nbCusG7qiNCtXk4ne6zMFEZEUKvD1awLKwN8AlhfzuuOJKfcgaP8XvycmGDsXuYJMBdGCO7UpiajjDCmHjNbQDFZuu9BQnmm%2FXtBfpturGr8KWSuOFaNhW9Va65MzVegciOOJGEp2MyvXocgaSTG9liIw97STyQY6pgE9YIuETKfi3o7WeCMDDJWv0RR42%2BITV7tOmhlkPF8X0d9YhgmVFgwd%2FYaRfdS28i3mF84zMf6nMY0dVV9SClncskBYudi2EpmqcTDK3Fnop3LVgcrS7Lev2v%2FRmbLPpTHc2Pze5x%2FMn9zNXyYzgLBnBBugXSbIOA%2BcParIWVB399EX5NU2gpXw827YOSlzuW%2FYcMil3ZZ6LIm6wg8dk59Bzj86nWU3&X-Amz-Signature=92f7740bfc70e71026ae79ef84c331a3cbd82bb996dc7bfc0395f38bc2eccc58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

