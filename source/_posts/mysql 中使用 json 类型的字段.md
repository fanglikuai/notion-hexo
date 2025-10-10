---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRQQIWW5%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDwPCedgZSa6LDQyZbdPZ%2FZlnPxaUpspkx%2FIEi2CDwDwAiEA053CziBvsxupxkkWvviwa4dcsqnoFf0EyrUysLT5hCYqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPew0q60n3QCZFSXIyrcA9ljNMIVlgxb9NeC2z3uSfmMYRmAKYiabef94kQZDVDGHL2fMwItLq5%2FzWo%2FYf4%2BudFee9ABNA%2FA8So4LDlsm2ki%2B4PGQ%2F8B8W%2BnmvG2leyHhqUmEdDYqE5Ckj8mX2cghGGj4CGhmmw6vKHJIeoHYzHoH7Mea0sp0pckEqzUNuvyctBfAFqEg4K14JdygHMCJ9q9wwfKf%2BGeSQaiRKRrqniSzBQuh2OCWk5vtPpfWXih1lhMChoWj25GNiQc%2B3dIglF%2FlfpDK89sl2vxgHqxt4LCcylpHoEjs4%2F8odxR3RfKNMvVNl%2BZUmkJnKp4BR4Sk6z7OLhlkl%2BGzHhxU7NwKwXcuesAwxrUQU9t3oIe%2FThHzaGwFhs90TV66TNYjlb%2FC62E3O5pDY5AatpO5mZHo5AzZJlGuHBjVCTw%2FuKZ7fBTqocLpx1SFPLDn7pnxb%2F5saQWfIV%2FzOfqBvNlTjkH1yBC77yk5dTAd4w7tdaNRz78%2FIHlQyu7vgMYLfEKljNfSJGCRShCzwEfD8yGWXz1IDJd5L%2B4mlm79u7fXkbMKT0bUWBByqKz4SIPtD3DyuftcMkdLl%2FHorLCY4YRCLUPQW6GNry8tSOkzWGoZCViHKntObMzB%2Fy60oCZyFNiMMr%2BoscGOqUBUVwWj7%2FsXo1CkVdrp4gJ5fEWbJ1EOdP34bMjNXw%2FNnZ%2FH%2FkZROg55tX7Kx1FdBY1m9zp%2B%2FSHIX45XffjuOiBVpR21whtW5xYWsnLauPqytNWPUhg%2Fx%2F3quFh9GXlAmNgnDagXMSMIDXnKAS5Phd77rIuBMzgxAiYkbj2QdWoxIl2gOFSWRjfjF1O6NZXVB5ENKFFseddEVWYCIuRhNr9TSMD%2Bz%2F0&X-Amz-Signature=da0e48c25a9701c27d1779090711d1a8801ee320f1e82f2c8e17efd6c4fc852f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

