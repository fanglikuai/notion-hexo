---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5BAI7UB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW239Wj97oH0RVlV7hHZg75VKLVCcnOIQOKMvnCPAwIQIhALFEu%2B4VOCICLRZvE7svFwxgX5ei2TrDKMC%2Fis3iRKzIKv8DCGIQABoMNjM3NDIzMTgzODA1IgzxWMWDQtI1Dj%2BGVKYq3AM5WS0K0Hfh8J372dbQgGAc6FuaGDh7wMnEJg34pwVLh0khHHq7O%2F8PCHVo7RMHHk462DcKg%2BKKTlI21QcevMiqbTnH9L4rEd%2BuajF9GY%2FZcOcqmhKGk48NznjopqAXT5TEb%2FxIhnbilQXsJnIjZJBgxRRjShCFjDGdcXaWk0QE1V1gTtZ%2Bx644sPs6DHvRPqtG3BEjOhOYHOxlzPdGXvdA1fcA7Z7xr6gU30ka2kaIOpk%2F1Di5HryxCdA1CUN96V2GQ4kwHNj9%2BP2x6PfWYw0hAFd%2FnaDkM6fxIAgH74K8PKyyY9YTsO%2BeBea9qJnVAEmrcLVTokKqS22CEsRogcRqO3G94FLtAoc5na5HAjw%2FDKJMzROqNFEuf%2BhgOp2TpWBaNIdbENFg%2FnvloyB6yNNYiftsIFxxEyflpA0dKUK8SvcDyHcl%2BEf3XY8DLlfmkuUhv5pXYemmP7gnrd35sFxXy3pm6BZfaFLDtRBXGCHdOuGIGuuLXHtfgBOEl1tWsTHs0lHfKZB6FYBLbjqwhvhtzH7pXKC5TiXf5FkbDK1t1XsxUkE%2BbM21wp%2BOibuURNjcCOKDYbNDbfMFWavd%2FL31q8pRwWp8a1Mt%2Fr2POcW%2FZd%2Fqf%2BsBPPWKJ4ZTnDDTs6PIBjqkAUPqFXhdLj3%2F1gDJvv0yzbfX2l1DpJy%2BAf4BerF0qNWEV1n30U9dgIWKQfY1sRGj7mDqao2CKLf6cKhT5YDTLQus1MPXpGMQveu8YzqiXjnM7JzArhC4yYTqcvsUN4zWB3gU%2BjI5uNriQ84wDlK0awYP9fa8xRlZyKQGOTPM7pdES1jXSbc0trdTWyioognZdZ1cscqlWNUQN7%2Fba%2F7sZ93SsGNo&X-Amz-Signature=256a92dcd258cbd60c3aff8250f7bc9ac6ce9431f83a202772ced45f46b7f58d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

