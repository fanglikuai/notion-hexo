---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG235KQR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDL%2Ff0guQ40bP0GJ4TFA56TivmOCUNbARS74rFhlEDqIwIhALSXZND6Pi%2BW1WtIvJyH1YIgvyJ%2BLtbSmAKNLffLj899Kv8DCFsQABoMNjM3NDIzMTgzODA1IgybZM9iQJqc4%2F4Ujygq3AN12uneaze98miNpoNqFsRt3UFjeRY6htyGzTyeMEmqUnHNRGcf9f1LmzwiwfrtEqKM9Arp9MDifIDkKBrkYJAd%2F4DKRkXO8LXuOZbUNTm5sNHHVLpjtDbBNpty2dyNHO4zYSvpB2UUV9yLzGZDtZQviQRpB83eGqXj0Q9i747oCIZJQhCjeBW49EaM3b9yBpbb%2Fj2WQwNnPMPE5e%2FF7R78ZycL3v8x1lsCIabNf9Y4QWw9Nclj7yftMGEFnPTVSincidtbzPUxs0z0tboGEkFt9IFD6GYwBfR09oT74vlsODI2Ua%2F5M9Omz4IdEn16JGRh7F%2F5RoNC0%2BW555nOPrbrK2%2FsjV8ur9ZJOMUwPLODkZjD3imUGTEX8LbZrBvDuEZORRKjNwQiVsFUtV1S3OZdt7Z793B8%2FDivok9T47uqTBLe0ng%2B1aNUvZGtf%2FUBGpS%2F41i9dlcfOizh8gA2hlyOskVWJHcnvbOQLrSUufLdi0CC6XCd4JPEnkJ1JiEv3%2BL%2F%2BXzMpwRc2JOxEx243j3%2FvlmGB1UjDv5LNlYUNdF4QpiNbq5opWpCaP%2BqArIIzL4hvWohQVQaoCxmt3XLyvu88q3rFygdZDkOAo9QKzjRyzVpLktJowYu8Jd%2FSjC%2B4IPHBjqkAZ87jFImGZK63PvP2RGP8M73KsU52EFUsoNQFeaLtzaZoxx4L9%2Fsc1%2Bn1je94LH89xpvWSarTEw%2Fff9J06LOImEUskb8WkJGLs0hx8hKUr733%2FIpKKHOIBYNRiG%2FDxsK119qtFO0Xom7LLW4YLS%2BpR6%2Fu9ackkiF2xJeFIbS967KnNJEIyCqH2fuFH4YDjQLzLpLJSue6qg4KJ3GFjI0dUI9nhf%2F&X-Amz-Signature=5b8fc2f204b8fe09540dd431a3184f29b228ceb48fda8340d68419b9f609140f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

