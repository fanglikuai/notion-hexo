---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVQB5FCV%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWi4M7Z4YZk8R2fuidzS%2FCYTrEfiyvRs%2B9M8W%2ByVqO%2FQIhAPAFK%2FgcxtELVnN56EFaN4tKf0WA8F4VIIsf0oQHycVCKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwjAPm5Vun2EsyG7Pwq3AMRT%2F4kjUZS4cBNunTJgfXilRqQa%2FLZH%2BveLTE4IVhnNdQACd7hmArfzH9M8l%2BuLz5SM%2FeJsOiHeMlULZZclLs2Hsz0Qld%2BLCQvOXUpjjGpJspdE9sbbgL44tGWsMdv%2FG%2BpHTAL2VPoduV%2F5hIlUAX6F1dlhmoTOJKd3iyVX7kfgAAJLyUWPtd3erU%2B0MjrQ5f4OBURvMgQWi2kNhC%2F4LFIbtBAKhizYhYPWAd7E0Fo5Jh%2BBrFCEwU8eLvTXeVFSP4z%2BRBq83gGcRgpTcXZehowZEPj%2FNbJuj8gmwMIZn2b8gmQ1BB%2FVCotLpxgS2EdFJWtADkGvRmirSAHkrnv3gMcK4ETptUW8YCRom6NhkiWbXkwwa2Sc%2F5abkLqRKndTWfuPElVYvTSWW2KP2bdp0eawRMe%2FLPfDg0NFBa2Za%2BW9MYYMjxtIjPIptvnymRadPik2aPQhlf%2BCgIHNEWx4xUEGd5RuI1%2Bcpl%2BWT%2FRqCcI9CGPP%2Fgii%2Bm35WMAqUdcGbtd2q0ez%2F3z9kvI22Xy9xhDdAC416oZheXKSgE5JvdV7nAmym7cxMykFtkqoFCJgFSBLNc8jqJgas%2Bd1cQKuRJtXZVbrKkmdLORdLEZozY2Sap9AeEqz2TeJx43NTD6%2F6%2FIBjqkAXgmf6E9B9DirtiG7VcHdNRR5RwebzMUzUU4U0CU6dMiOyNq%2BR7AJOsBSr9LjxyRNRSBs8U70cYCoWiZhSB2uj1YmQiiGwS3wWq9fmkK01ZeqiQUzEKwFa4Yh%2FQo4nvIbo9BVTMwIUiPqrEUZs9j0ePweWXtw8oQ7Bw9MF7%2BQbwWf%2FNnkhmo17agMYjUEjN083gEMBHyqSMouEtirUDTHQaNsvxB&X-Amz-Signature=bd2115a219fc49b92524822034005540f770c08aa99d645ebacd6e8531ea23d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

