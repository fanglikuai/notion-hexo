---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYRQEJEP%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T090106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQDtMoZYJh6%2B8Xe20FQm3%2FitAmpPJYguYbAT7ygjJnVsXQIhAPx0FESnemgeSBOf21kE6Kj4oV%2BO8JnccA%2FfTZ2X7Q1vKv8DCCoQABoMNjM3NDIzMTgzODA1IgyLrmfv7O7UXy%2BqsTAq3AO%2FQngz6N%2FUe7fAu6AQdvvz3NIfjmztpQQdvyU5VMspBEZ5c0hX6KpHIHBApMcscDq%2Fny8Pcxen3ZFz%2BcLSXo6%2FuNbdiUOz%2F4dqcEIL9qDhhi3VTBELz63sYK7xC%2B1XoATz5SL7aXY27U3SndT075A%2BZiiDli3%2F%2B68OMA2ZDnpeyho7oivfpPmDpfoDnRKIctYqg4TYcb0VfkTBcCk46n7luwj6o55giZmYDrFBjlx99Nv8Vh9wkURVVT0SJpzqA8UQ4z2Lk%2F9QRPXUibCXfnnXg2Fpy94hsQ%2BhFc5xBgWuw%2BQiARRIwfUDU%2FXGoRnqcJIi%2FxxshS5LvxjYjZ%2BriKqittmHRnrM77lsKAwksfo4cX7BIvekzz%2FhwW%2FSKGwPeag40lFyzq4GFiaEBqJUk9%2FU%2F1dcZaonuwGWSlT792E9%2FM8VPmq3pk3OevUU2Yy7ZXxiUuWldU2AsdUf4RLnEqYs1NYwePuRMFzVnL%2B4IID%2F632S3kXqRefR8XfIX6XmToOFG5m%2FlNc%2FxBdfGioORkb6Sz2RBehQhfvMxijAvBkFHKqShGfsJGpHqs6UeUnhBYPSDod4%2BBWjEEkXasT9d3bcNuAQ3xtkskWzvVqVurFCqK%2F7gGL92PtKUMQgZzCfuOLHBjqkAVLzIPHLVlItXuhRDhy47EABnfn2wCQRjWTe9VLLIoonFjCU30jKm%2BSyj8%2B0E3HcUaVsXMdUPRCvqHDox7kYEgA1IdQ6zaCiogHtiDOJw1uFC78dywE6GKU50iMGVUyNQLOMJbtlVTBw60X4gBSuuOIWfLdsnkmjJ3qCAzRauQQHm7ae9thLKvnd1wpv1gapzeR6QsMn27oL3AIMZXL9acH2TPg0&X-Amz-Signature=79a9865c81b75388f19fc68db85a9c4b1701f56d2652f7eafd2e32b0a26c5790&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

