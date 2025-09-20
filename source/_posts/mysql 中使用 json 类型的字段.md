---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIXNGJOJ%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIGl869kOcyKauzSHBAML7ox%2BcZnU6Ovu39gedKUUzftAAiEAhw3SNnuyDr0YCfpp8pP9fiJGsfqDTAZZn4QZjhBzC%2B0qiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBRR8Bxz%2Bjbak4L65SrcAy4A7KjWYn685bc6DgEjAXT9uesFHwLW8izqdFbE1FeZ64K51bjScukRzXx%2F59%2BgW9x9NzPB9y7ZSWC87449q%2FFD64Q2dxu9WjK8LVL0NxLsnWmc8cVa4uX1oKwHcYwDO1NyMevyCnzkeF0OFqOpL0%2F385bT7N6x%2FKdWpTiGo77FzToiYIbgpMtO151mthb03FmsbBOA5SeGrjUcxMIvlYaB2DQ0RgdkibWqs4%2FBeMZkCGCadLyAPhrlFUV5SDWwXykwuGw7eRlqBdibPbzSxrCchzoGjpqzMafbVulmuJwVO7xAX7xdLjCTqYZ3GCmW3GRcnxD6K1p7ae6xbFFiGfDG7qZmbMbSnGuJBZVIFk5imtfeQrdGqddnBQBtA7eR%2B7Po4u9%2B832OJXzd81IYORM39Uvv1qLQutDnmkCSBmRRPO2iuFwMNpvLsK8needuL3MFHHIJETE7tEoXKE2DR7LgV2pRNV8cpEO4Ios8QJK0%2F5N8bun3077vb6FSjKflF%2FCBXtPVYZZtM1W9AESrWLYihDWaFcTNBVxJbru3b%2FCoIE51t8wd8VQIeYRn9SJ7Z62q%2FBFN7pbh5N7h%2F9f79xWFHOcVa69mJnxJw2GjmMs40PYzp5oclvAWqftwMIOUvMYGOqUB4n8YSOX%2Fa%2FQ8468VlFwSehCA%2FZWwrnDLpFxIPB%2Bb2Ur5%2FrDG8kRrgC638f67kMfYKIhvFMg7XL%2BMxrfCdY%2BROIKBKWt1DOsfRUCnjiwzq3nPvHgd%2F8xMtjqlW0H31VLOfS431ABENKLueRI9Y6hzM574oyfXRTnE5tG9fochUd3AUtS7svVs9MUEX66L11vDkPCzZb7bTaiF2OIbbPsprc6Clc6b&X-Amz-Signature=d01cd15619950ffc1bc110f5a18b0aa4dfd57f2a01b85ddf4f655a7b5365239c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

