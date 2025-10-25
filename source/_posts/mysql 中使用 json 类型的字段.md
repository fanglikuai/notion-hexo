---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGEQKXQ4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T040055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD1l8ukE4UhtSZHqPGy7qySa60IzYdrhMFTNIhBgGbiUgIgOo2GHR322DYZrKCWmTKyByfxpgjUB2yf4hGDM35%2Bffsq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDHSDFMLZS7ikYv0UpircA3X9cT0ibd7zvj190C11Y28Any8Vw55klJp3AuCeN0tRv9Aqh2FUa9vgJKPENfkzJ4hG81ydmWZMxZIRmeIy3d7X5F0GvmpG%2FCeRYPS67IESSqZ29PdLieAm8%2FWCY%2FqHm6BfdSkwtEA%2BKt9nY4IlkxchnCaqwDDu8RI2lnAfL0H5mrWt1KVn6Re918nruDzm4iga%2F8LOkkh3Rs2PvHrUt%2BmPdD8NbMrGh%2BH%2FqPMCSlTF4L%2BmBmGHTGqYAIt%2BFJ%2FyCUcceVRvVckD69DmtHDxAoqWQmnjdLGg6D03erc%2F1fH0gm8elB5uLsylMRuHFY%2F18%2FXNiVYRQUgEH6aoKg5HgT27c7NoiLvNh8pb6UoB6jVoFLPar2veews%2FxpBgjM1SMCI0vswckpNMpkCLcNbBN2LJMCKz3qtXv0Gi3dqQ9p%2BqF%2Bdp0IPzrdAbUvnhAXKBvARIJ%2BqyioOr6BTmGMDWvJJzrlF4QaHNbzRvScawIH0b3ZWqoZuZ4p8smKgJfs9uYka20FueIvawLj6nGGbbbQt3rKMZxyHthf93J70AYPMZaiGFOWVKCmG6W9yJkOQmZ9%2FLLGEmOAfB8bGN6974pasc7O25dCvI4lvn4EvHk0MXNC6RwRFFohvT7oFrMNuM8ccGOqUBd9ordSynTgiY1YVkWd1CjlqGTC%2Fj6nrxEEUAwl5p37d%2B1SPqHW3uvp0mwWKiu15sjcqPD%2B3B8W12iqlork3WIo5Dcf21l0ExVOGeUvYABO0BNr5IddeXM8sH8u3RX6%2Fvz1CBsh%2FO%2B7IXNwP2sUgE%2BcEGSiJbH6D7QXGeEvJ2F23almrhIQW7feucJhLV06Lz1G4PSYzib9WZA5aplVl5k7Om5OoJ&X-Amz-Signature=6d1a2988619ee498e2060a0dd356fb360bf69e963590ca1aeaf8c0824018548b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

