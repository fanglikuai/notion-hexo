---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFX2NTGL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T130039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7YXZXcJj1Wg0ybLIYNHMdl8Qu5BDPtFVXDJoU1E4W%2BQIgF%2FoVgRQ0JKDzgRht%2BOvN%2FXNjiTnnr5oN%2Fweb52aNvooq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDJG%2FDVOcxedT89CEMCrcAzCUYnYyhgngWxmiT3kqLWdOi5eerMix%2BOwjLZtisNXXul43ulrLCheVZ%2BobQsRkp5PNx5EwqUmUOdbCX9yooaoUwJzRqKtU44HHJ53Sb8Cu9ZGWIMhyWqeUIOdejndA1WAD6qy4vgk3VSwbNVVq720NVVtvigGNBPXT83HT4IaM3Sc2Fltrlmlx9x5cJq1woFGyNhIEfRss6kPltvTXFmeH97ucpQnFvCT4gteYPfC%2FBjqVdg0f%2BPHCiKFd31BBu%2BZhvecC8vo2%2BE7%2FFXGMRhQN3Z4dWNUFgbxlcm3O%2BAa1C%2B5SH10Cu5GGRjCDXFGPNrTXsUTHkd3wkPvbQ3I2v32kZSdvTeqX8TmXewhduE3vJUamrZP7g7lEJ5B3dvI69cjIIfI47FZT06TMNhescEnleWYn07Bih3%2FSEqSONJ8CeFknAddnFowSoWEnRMdkZtNpkZIZxR1LkG5Ylv3hA268WyQiDD86HSVGDkZk3xP2piEVuRuELog39GqIXeye5OUxQd1pZYKRs7KUC849ml8Empdmagql78MkFD3lxu8KcBUCO4flg1yDGIVOD%2F6DUkltuOvC6PE8X4SGPNLW2sxMAjFCqhtLxZDepAfckvgT5Ypn5C4tiATWcmvJMNXCosgGOqUBIb9iIlg0M%2FL45F%2BaSoJwCjb69j%2Fz6e8B88Na3nnweYMyaPlClxj97OuVWieGZaBmnTslhvbwABiTtoSrzI9I7%2F3X0aic74lzJDBg%2FRlDDACmLJZlTK3xqyhn1TGUk3VCaceSRU7YR84qvKmGGCoBUymWQS8N6puULJHg8lDtnT7VVylFbsRNkqVodL1yj%2BPRPxuC1UeGQDdi%2BFxxd5IIYy43Ymlr&X-Amz-Signature=2bcfaff0c7d8e58cee7147eb82d8d6b98354a8280b6a51ba47ac093625bc0e41&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

