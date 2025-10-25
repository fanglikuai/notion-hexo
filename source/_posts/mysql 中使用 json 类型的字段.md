---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WO2AS3C%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdpNCyZN0A7pQ1Ot7NX7Po2%2BLKJW9%2FE8TS3QCBa2%2BIOAiBy702cVrYmo4Ram4HpoAtdxHPW39fbNiLtaqHEVL4fgir%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMFLOjHE%2BVDbV0Uf9CKtwDL0E8wqLSryPSeUOuO73rV4yIlr3P%2BMAQsHudPBy6HnNSgalKccMxOJRQJaYfuhTmPeanVUI3e2GmijhTCpetRNL1EQA8cfH36P1I8vu17W6K5e5Jqqzska37xJ8W18bOBcxFLWE4LaI1eRZHcdQlWWr1uy6xABxXEp0xLg2tJWpN5QOsOT7xNQKw6mTFobTBU6IbBJo9NKAKDngzaRMDq9HWdg4O%2FabSyw%2BnNSOoyeTtVUJxMLsh%2BVa%2Fwz7l1BbkVIUo95xFtsoMnANCrwIOZcsI1%2BViTSjZnEqKWzApZWh5gxTmCCb%2FpOSjaI1D7FWyKLGJk2VLTFRVePiGipYOs5ioPkGUx0E%2BYe9WyT539cz%2BMx2EO2fw83huBwje1GU7KchJKnT4dW7rBtE9LU0weX3a7uew41d8shdGh4J3VfDj4au%2BnOTBm%2FJ1KJyhMeyWwcfBP3crkStigg%2FZV0ORwdFe7ebUlZuYHkkUvdpbwR7u25SvoG%2FeBYajRQ07iOIoXtXzNwQcfDrtmeOXRKJz8EEPw5JE6cz8wtJtND%2BIc5WfTCCfRVGFwx0sHzx3zOZzuV3FoyUoJnYvRKuz1F8day8HqnpcOPnXs2GZShCCd%2FE4Ls5dmH2dlORpRrQwtfbzxwY6pgHNbzbCXIFb%2Fg2S3%2BSMyUBy%2Fm03e1HT5JGoBztUetw6KlxpkmVQi%2FHz33XWhyUFqWMwEP2nVAMBdNnKO7Wt%2FaEMVc1Mlreu5UZSBQrOG8WZbCaPj5iqqbxSksnNmXpEBQmJEqK4ODeVnM7TNX6xz4ee0DJ9OOiJntciqJH3QO%2Fp574wpMf9QosVMDFvmVzwpA3IZWtIGxw%2FpAd4bU2wEXiafw%2FtdoR0&X-Amz-Signature=9a745064cd75848e37735b0498539e4c34bdfcc5013c5cc073b6fc9ea853d150&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

