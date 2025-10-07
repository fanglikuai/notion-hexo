---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFFBJRWE%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCID4Wk3E3KqnS94Jj2Kuk%2BS46%2BTevDlxWb612nlvq1opOAiBD7EoNpz61vjhkU2d02LOYX8vDDQiTpLwwQpsDBY5DDiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbejrq1rOAmlSA%2Bh4KtwD6gPH2RDgD75VqD2P3OvPgOfJsA0%2F8wqzoEbM94XMxqnEjXTfdGGZc2hoouSeTGmqTySTaF3cPklfonP4RXdPMrtNjXCNXJ8A6PX0OS8jDey9rxJb95ryWcEN6zE9q4GcwW9tSFf%2BycfHm9Lsma70JoMmb0POSd%2FJX59F6cq9lyONZaTO880KMndD%2FbKI1mOZtgtFDTgXMbGBxNNW7eyKlXBD88GSrEtz%2FDz4MmtQZu6QcsiCsj0rmkF86Xz2RlU4DH05w6QekyqTgcRMhnQdH10Ei8KA5nwKo9TjjQfAyUwUbGWOCPAnyFqpHSqPp7Y38cLiPuu18lAtEJtmARSbBWu%2Bq6tWLs%2BgPhFGiunLLtm9xOVZqQt04FLk4LMuepFI9ODBqh8FvhfekGvA4d%2Fw0mh5H7uc%2FDqJlg4o0jPF7qlyEH1UYuFHaBd%2BkaRkHX2xcC2375keqU%2FYs2fCyTHX88R1KMi5Z9FI6rysKXXFp3RSJUrtkLKLV%2Bajk%2B5pb%2F%2Fja9F37Sp3IHV1%2FLLzA4%2BPfiVNvti1aN9tM75Y3d0wVWPuSvWknbZRHX%2BFjPlNOzlDKkZ88%2Fi6JdWXF0%2BXx%2BRs1w3g7vD%2FlIaELsJe3GxzvTZVC%2B2MrY4ltd8eq3YwlI6WxwY6pgFIdvLTk76Nx6NgA%2Bq1whzOMMcGIOmtB%2FzXQdRKJ%2Fvqlzw%2BTjn1WdMNopNxSNHshjSFjUinoIOiQketo8%2BupibipSgQipMvxIKKks9mDgd%2BsSuBOsDc7F4qqfLwyrY5P1Q5NM%2BCM5HgwibueF6XSwYuawql1ytFA5iwtsPFAOkSM2sCr0k6VSw%2Bqs3wTbbs4ii%2F2ucMHd762xw799hy7xZ4B02LWgnh&X-Amz-Signature=e34b60d8c2cbe388d5086ed86ec312dc8701943a5e318e2a024ca3c48b9229c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

