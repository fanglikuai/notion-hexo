---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZAOOVG2%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJIMEYCIQCG5lLmMBIVzdDLFEiV1VTXTGt87%2B9knWgUYcHhOGeUnAIhAJepOpvUBudsZ5ygf%2FLyupGYSCBa8cD3BXUPlzncze%2B2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw6l0aQn7rSXx43fJMq3AMoHlFe0Bw7bVdsnBIvBamCkbMJo3oKMFRuZj1sa1lPok99zWox2Kk7U0zgRmc6U27mLWRFxvbWLyjBAmE8opFQhXpHeUSFXtUrdJj0XHvEHDr28XHI4a0SxRArmwTslOeYT%2F6f%2BVZhv%2BDfYGDxeXhfy1DuZ5Z6gdkUau4jpdzQ8Mrb2GOGQNY%2F6e0vXCWLIH%2Fvsort1mNFPJ2O71z9JMN9918Cy7hFU%2FAlUY09SPavI9jpvCV3PG8ZgHx2HiPLRJUWGLWv1NNho58kTNzPWj9wMLxQKFG6W%2FzLaXCzA%2F1AwtNBw9AHZU98oueMyUHT7oGDW4Nce7LnQbGJRxMDtSqMvVWLBf1oc7tLzWUtY3ajBg4fir5EfgMzM3Uyy538fX1OkyyO9D1wSgYLjvbP0TnDCWjsfwdnHwjSQo7TheVhnqNchKRGTUGxRss7wROZjRZybkQgDe9oyoN65Ym7OuRYtycQLcdfVnvageoQK%2BMw%2BHM%2F5%2FDejdnz9svllxtSb2wG1eU4LySJx8sGag326LfPRg1N3gdvVeVNZxUraZa4SqIOTmSVeOYSlwfH9XoKhykiEXdW20VZPqhSFxuoTfXXXGqK6mZZAOV8QUPHiP%2BSZWk5Lk%2FKzyUxGAY%2FRTChgufGBjqkAcAjxtT9JrTZMZwSWzL7vcrsKEru5YWe9UmNSNrayje3CzgcwaarVHYaJWQxUiP4PRuIo4UGUVf1W%2Fy1pE7JTO1ruLFva0hROik7IOzJcMBYot8PdtUEqPXdCY77Vqo85vFwEhhBwY3PmC0gSokf187s8SCfwzw4YDUcUroc%2Bkd%2BxF%2Be5ndafg%2FhQKd1NATnWo8GdpPjhez9ALehGMEUyTZO7tn1&X-Amz-Signature=3f860a1ec1e71c00abc8595cdec670e8553bb99a0b8ed3992e1985b65326c71d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

