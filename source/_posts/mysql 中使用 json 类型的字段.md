---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJ7N2MGI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCrk2BgMyesf9v5U4b7vb26b7%2FpCGS9WJdlO%2B5IuMuXQgIgGksOMbkInpt4sCy7nMYrd63GvvUIHJbWmBYWcmdlgJIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5DKEh67xzkxo%2BdESrcA3LQAChjL1Gc63wZxZyc5HOdyTun0Yt8NGeKQgiAqn%2BEEMgB5kgSnhpiZpcPuybFWdPKtmnLiGPAVUZNcdL3aAhcoEB64O2ApE9w%2FQfuacKX1xPLhmWX0t4S4OyB7e1A0CWq8uk1lqvi1yJi2LNz0iFxCT0tp%2FPcQTZgNTCKwEveX8TGVTD3B33KO1XERjkR%2F8pmwT6iaIlLIYGsat0x1yPMFarCHbd5XZVnCATMNpBY%2BeX5DmT38Arvzjx%2B185oBzInZGSqLIny94xkyNOz2aXPc%2B0%2BRgoG6wp2OhBEfE4mYzcpxQJHrK2TEncbJGt1bn%2BKvWegR33y4r1FGf4oiDhDh9QOwZHbr19WhkhCKjh9dT2fjJQojODdhDgcRWWVJRJ4qL5f7GAn9IL5pOcon%2FglKzybWkFi6eH%2BuGn0WjeAXa597cy0ULprvWKMBNg%2FeRsIdhMktFKurvWQ3MsuLMyPbn5w38%2BYIDZ8u2TyY4Whhh9jjRtYWTi6LwO32DLL7RvLBa6B67Czb3vkYwM496fMTFfHrYYiBDx%2FRh87oMa3en%2FBPp019dJvQk1LUQmf1kq0ZACEst0QlxGW4QGEHugRVEvwAPXLPvSbcpE8BklBr28tR83IP9pvWEFWMJ3y%2B8cGOqUBzDpXkp1Al5shKEsUgwlj1z67j8e%2B1lA6uxeDioeT97PfA4GbO6QH4dqmSoq5O6LaCVm2spiU%2BmXdKhN8sf8c2QbwGQrmbggbocMQLbJO2J7Wp8vF12nLNgUjTdEVeZPLdX%2F4VwLcr7%2B0EzyNRRBTlg5C2y2be%2F3FTSRFa%2FmYNQYBkwmFQwf9xh%2BCfbyncOikzXzAoKxRKLM76eF8dJiBajKeSQjQ&X-Amz-Signature=75fa3e07ee267064ebdbca08882b47843d5e2ac9bf63d3d79f02d11afbafae7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

