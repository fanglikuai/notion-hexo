---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZJUOTN%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T000036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvQQ4o8N805BqL1GhU2LrvMd%2FPt1Jo0Ry4sMUKp%2FgD0gIgSgmWlE9fNVn%2FcgO7iz27ExB6wRjQcqektJuGlPO1fskq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FfwBggwz%2B7GgKfEyrcA29E08SnPCJYDc5SHn%2FwyBczUxRBfzst%2Bz4BC6KgTrn%2B8nR3J5CiUEy3BSJD8Fb0Z4HMIh4w0aQi7SxZNej1dYOWvVlHoYt1aGMTH2r%2FNdAKJreYI9DfkeA6IJLROF1LlzpYQGtT6%2Fln7JxXRFNi1cePDGnMG3NOeAlot9%2BcNRifEOEh1NiZJ6E0b8HSo42ygbm7zPfZUKuehRyc8WlPnjHGASJ8gvg5UoXFrGYZjUGTWNGVHsJFcf5rN7vk4US3nl4RRGcwxVbVUBX1Esw01dkZovgVcn1%2BHsGezRXKqRFnGyl%2F45CKYmoquS3Eh6o1R9IdwwwH6Tn2%2BbK8CQBoATk6DsXq4%2FG2WAUX22GYSq%2B3eGjBlfLoO3rxbBo5uJQG9hUb2oEUQ4AHaoKmeJ48lSTnybta4R50niNokL3mO0bNCWjGJJWhXmnfYYNeyXRPdVTV3%2Fu4dLuMlfuj00fwPB%2FczkRJZnJ3ogxi%2FgpfmtwcMsDoyxW5i86BjjJgDbOTolCERRlJ1A%2Bh5VTJYaaeKs9h07b3Iwb5QucbSsNetRmeh4YO8d3rO%2FDPfsbcodDoE7%2FZm5I6G2pQEDbqeiJuo9v%2FCM%2FNw3LsqM5bBm%2Bp1uFrcWz8wbqVLgCuDIgyMK%2FhhscGOqUBH0qTMoNCJLt4%2B22qfiwIaj0ZBV4B3m2VfS4BLeB3ttzrIgCYSCXV7va64H3hPX%2Baudt5URdbtLHczf%2Bma2%2B%2FaQNruPHBBrbEVx1Y62RwBi%2BDFoEIoOuv%2BiSOF%2FjkhymXfaUbGvz%2B1oGyvvLG434CKKdeNFAn9HtSVKhAyulpaNX1vAjSpwOfPpI4zs8qXgyewImxYpmdx%2FVORqy9sYHIvZAvN0iF&X-Amz-Signature=d5dd200cf076a0f627b6e76033231ad28e059704cb09913473e20c133284b7c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

