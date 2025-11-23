---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BRIRXAZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJHMEUCIHbTkhMlj21EjTIJgd%2BUWbIuGCvakg41P47a%2BTlDh0S0AiEA%2FLvTRKrARyRS5RTHPvDGkC4%2F%2BT7TL7WdilIYu49iI0Uq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDLGz1NV0WtFRWnSAPCrcA07eh3%2BivmO8FFJr5ZFLPNdjbnjApypf9ckUSNAIwi7NPRsJiEmLPfku7bMUZXviYZNLNUmcX5cIZDWfpnOFgoLa%2BGK%2BmGyPGJFa1nR4pKGzJ%2Bn4LYulLvmtIs819zu0Zl0pCIyLBDyV%2FpFPhf0SjWpGcZbP7f4dkE5yw4HvwzHl3wf1LWTm5kCsamh2wYTRf9pH70QLhgxqL%2B8RKia1z3F5SGyr3JcIG1upva0VNQKxg1ysiDJ7UP%2FPYgUnG9srl6aYWXKzzX48t15qfoUMKOwhC6uZtHK837FkVzwaMJ0Qf0YPz8CRhD0U%2Bf4DlQlRVcTApn18SLQKLlu6RmozGm6Qe5dPMFJMTqpnkh4dP3qp%2B8PrRnbai1PPxVeRtOgPmPGVJMtUrxNEKosnibNih3CUKhqyvj3lNrw9la3r%2BRnej%2BneyFqlYWGGeJbxs%2FE5gOIy2OFdTUXM4PhmaILWmsKFLSLXHM2%2FDFUuIfp5UA%2BKnOQBb8BmTikbNTQ67GJIlcPMFH8yGG476xaEyu%2BYnOQiXHVTWU%2BB50zOVsSouL2m3xHM%2BN%2BejS0W6ERktoQfgGaj1k%2FLIcsBBTkavPS%2B6rZU0MzhLL73S8v20JI2kPvS7fCV0G0hJDC1hinwMIO7jMkGOqUB1fFW6LJkFvEi4KlM7xqihspeiRFiBVKO2jm5sCvMKCy1TdvbK6m00UxgBNtICswUR%2B9QJRfT1Cet2SOz23zHx8HIG8urQJWwM9fhoFJxm7pMnbE8ImODiosbPTZ6Im4M2nvmgeuuYwYn%2FbjA23dND2%2Bc%2BrOaQFZHn1htZmCQ1zepcSSfxcLmby5tW4SuLpSief8SZCSnQ4Xb4fgD%2F0n7QBBTAqFQ&X-Amz-Signature=66c5142f5bb25bf237081b985ab3e7f3355a0bd3ccab90970177e97d073b64df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

