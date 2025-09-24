---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTPFGKMA%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG4DPatU9%2F7xa6Bu893p%2B2nc0igsCJLSgj1LIVvTIrAmAiEA%2Brlymp8YihmKJ6SQ%2FzwhIujG%2Bj3SAOGXSdg5zi3MdgUq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDBhpXFNab8JcfUGiFSrcA7wy1heiiR%2BzyGZDqsCBaxCtGtVS%2FZYw44PbBEidCPzBNtXcmuOHU3jz33VmdsoOmQcaUFUJ2VFAnsS2SuS5Hg8sSSGLiSTWXjRfycXGRuckde%2BN5S%2Fkqo%2Fr15in8nCVJEezzMKPxBZQuk9v%2B7qIIMpbgCPBws5dVOfwtYuHGfM5V5sttwiCGiuXd%2BSXmuDjGbJQCmNZE2%2BBPkIAq2QN0f3uDqwp8dxUAc3RQbPXX5cZSJLLpQmjfqxJDAhKOuXt1p1H2Jwqv0UdQbjVQ%2F75p8IBN2X%2BYpVSstCJkXJ2tpQ8xT%2FOIkVcgzDSxMp2j%2FlDDxtehCQJjCP2ayRReTl3XBev%2F4t3h2zJ%2FH1gu28SwS%2FNNY65%2FmwDEpEgyeMhL61sZ04JMoDG7kaX7bdqZ5r5LTFEMXhA%2FB0c65XZ09puvCNRO%2B5YRWoW3nJ6LwxBS3um6W3dCpb%2BMwjzkBGcRI9jX6TMrukfoMkQF%2BAEQBzN2Omis6Ica4kjU3FqpI5OkdRIZ2E57aAqmQ%2F5a%2B1MNK%2B4io8vRxCitZU%2BPdzTDK7E1MBhiec2rGljzJJAOt4Q7eCJFMn%2B1BYscsRlscPn%2BN1a0VQXwBx6hCaLVzVA2GKKhR607qlq8wyCqG75El7cMI3d0MYGOqUB5kQfaaG7Nk4ULFhhip%2FhnbTsDu1D%2FoTFFFrSAJ%2FU%2F7CEcRT%2FBuYIvfVxyt0GTwvUhe1zQrwCemY%2F2ewG1lr6tOmJGHJ%2FcWy3%2FuiItwjIeSoZdAF9SY5yVlp37AuejVQ33XCmbNseahfdG6Yq7uz9lQ%2FOkzYQjaAXLou1Va4j27uMUL2OjSbUNeMpHVxMMpZP%2FrvNTSa0eHwcB9pAlRfDXVhlTY7a&X-Amz-Signature=2e032922f1fab7757738c143f68ce51032f30f25ef5f1ce26f2b6ad1baa38f80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

