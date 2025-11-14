---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635ZPRZM2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTlIljWGm82KvvYZu3XVPf%2FOkfpSBbGa9YEOIrGvn%2BxwIhAK9oWhxzvZXpR%2FVC2ZeuS%2Bqj%2FgrzmBIzu%2FG6pIqBAZ30Kv8DCGsQABoMNjM3NDIzMTgzODA1IgyMlTA3Dpz9c2Sz8FYq3AM%2BwJPRqiy%2F4pfQF1EHqy0eoSi8VcRan5gmcPGVwYPcDoE%2FGejf%2BqhFeJF3wM5g3DGUvr8ZojDYSpFlMvyeqN8EkiMFV6okkvUgOUdpJdi0C3T9sWTBoKeDERnlRM55xnMxrD%2BrurnzTfZ1sPaIZnoCicPnx6A9FKH8X2W3BLt%2FNr82gtDrJGyfV7l8rK%2Bmy9VLPFowf78iM8n45PucQbdzGeI7%2FYoAYRer0AOd%2Fv%2FY8ksKrPHoG5PNZc6txqc6dX%2FU92iYst1WN5EI0lGYWENR%2BWrbTtun1TDYVdEpfyj7DEs93bpVpwqN7%2B3NYyVEXOEXM3MBnUgreoEHh9e%2FT8uGVuk4zXlsDg%2FQ4luWnkjCx06eWVaX9z%2BPNrU8r1SoKGqhhWAX8zC9L0x1hLSzj9%2BTTcVi22TqjcLCj4NlSAlOB6066dzi7kgiYSvm9idiR%2B3UJdZtBPGsQkENaetorMGSXhJVtOXeAAV%2F%2B6wgfFKGzSQ6Ee1e4WHkqF7JlFbgX6qtdFLzmPOpn0uZ9awCPbf6xZexrxje3ovR5o1J0PMZoxHpDV2FbR82PP1ms%2F573akk%2ByC%2FdBkRVx8bLVcBntvh%2BMLBZHmI0%2FG4SeangHX6TwN0ZaOM660qmfK0UzCS0t3IBjqkATHn68RToIySPMgrjfHwVBltM2qRch1O%2FS85UUi4PG149nCrcHDr6c2EWLcLfVbhnMyQ4EeLd%2BQjm5evxWVJ%2Bz8BS8oEjFxTNhInzJcYJVqkPOCcUG2mzcZiNHahLXZGfLoeYhaPwRpGhidnDFgw71u6Q4Ed4N6tZnz7NKjeBrJO49GhRo4boIP18ExI8YDT74NV2Xd0y9CbhsF%2BTtKxOmgLGafh&X-Amz-Signature=d9029ec435becc5d212c91424c79a03be8622220d51741e7ba6c62c0e0c3fe89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

